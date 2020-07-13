---
layout: post
title: Rails 6 + Devise 串第三方登入 google, fb
---

紀錄串接第三方登入的實作過程<br>
發現網路上有如此豐富的資源，還有許多好用的套件，真是一件十分幸福的事<small><del>非常適合懶人</del></small>！

## step 1.

In Gemfile

```ruby
ygem 'omniauth'
gem 'omniauth-facebook'
gem 'omniauth-google-oauth2'
gem 'figaro'
```
`bundle install`
啟動figaro：`bundle exec figaro install`<br>
此時會長出config/application.yml，並將檔案加入.gitignore<br>
figaro為設定環境變數的管理套件：[figaro](https://github.com/laserlemon/figaro)

## step 2.

在 application.yml，放置fb & google申請api的client_id & client_secret
client_id & client_secret可在官網先申請好
1. [Facebook developers](https://developers.facebook.com)
2. [Google cloud platform](https://cloud.google.com)

```ruby
development:
  google_client_id: "your_id"
  google_client_secret: "your_secret"
  fb_client_id: "your_id"
  fb_client_secret: "your_secret"
```
## step 3.

在 config/initializers/devise.rb的`Devise.setup do |config|`....`end`之間加入下列：

```ruby
config.omniauth :facebook, ENV['fb_client_id'], ENV['fb_client_secret'], scope: "public_profile,email", info_fields: "email,name"
config.omniauth :google_oauth2, ENV['google_client_id'], ENV['google_client_secret'],{access_type: "offline", approval_prompt: ""}
```
如此一來就設定完環境變數。

## step 4.

建立新的欄位給 user 儲存第三方的 id
```ruby
rails g migration add_omniauth_to_users provider:string uid:string
rails db:migrate
```

## step 5.

```ruby
class User < ApplicationRecord
  # 定義omniauth要允許哪些第三方
  devise :omniauthable, omniauth_providers: [:facebook, :google_oauth2]
  # 定義類別方法，找到user的話就登入，找不到就建立user
  def self.create_from_provider_data(provider_data)
    where(provider: provider_data.provider, uid: provider_data.uid).first_or_create do |user|
      user.email = provider_data.info.email
      user.password = Devise.friendly_token[0, 20]
    end
  end
end
```

## step 6.

```ruby
# 新增omniauth_callbalck路徑，去找omniauth_callbacks_controller
Rails.application.routes.draw do
  devise_for :users, controllers: { omniauth_callbacks: "users/omniauth_callbacks" }
end
```

## step 7.

```ruby
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
  def google_oauth2
    @user = User.create_from_provider_data(request.env["omniauth.auth"])

    if @user.persisted?
      flash[:notice] = I18n.t "devise.omniauth_callbacks.success", :kind => "Google"
      sign_in_and_redirect @user, :event => :authentication
    else
      session["devise.google_data"] = request.env["omniauth.auth"]
      redirect_to new_user_registration_url
    end
  end

  def facebook
    @user = User.create_from_provider_data(request.env["omniauth.auth"])
    if @user.persisted?
      sign_in_and_redirect @user, event: :authentication
      set_flash_message(:notice, :success, kind: "Facebook") if is_navigational_format?
    else
      session["devise.facebook_data"] = request.env["omniauth.auth"]
      redirect_to new_user_registration_url
    end
  end

  def failure
    redirect_to root_path
  end
end
```

參考資料：

1. [[Rails]Devise+Google+facebook登入實作](https://medium.com/@cindyliu923/rails-devise-google-fecebook登入實作-ebfb3170b0a8)
2. [Rails + Devise + Omniauth (facebook, google, github) 2020](https://www.youtube.com/watch?v=Dd8dOAL6WYs&t=2236s)
3. [Rails 5 + OmniAuth + Devise 實作可擴充的第三方網站登入(Facebook, Google)](https://blog.niclin.tw/2017/08/26/rails-5---omniauth---devise-實作可擴充的第三方網站登入facebook-google/)