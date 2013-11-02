Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-08T14:08:37+02:00

====== authentication ======
Created Sunday 08 September 2013

https://github.com/intridea/omniauth - Standardized Multi-Provider Authentication (has support in devise)
https://github.com/intridea/omniauth/wiki/List-of-Strategies - seznam sluzeb, pres ktere se lze autentifikovat - google, facebook, github, ...
https://github.com/intridea/omniauth-identity - standardni autentifikace pres jmeno/heslo pres omniauth

https://github.com/hassox/warden/wiki/Overview - Rack-based middleware, designed to provide a mechanism for authentication in Ruby web applications
* devise stavi na warden

===== devise =====
https://github.com/plataformatec/devise - autentikace v RoR
    Database Authenticatable: encrypts and stores a password in the database to validate the authenticity of a user while signing in. The authentication can be done both through POST requests or HTTP Basic Authentication.
    Token Authenticatable: signs in a user based on an authentication token (also known as "single access token"). The token can be given both through query string or HTTP Basic Authentication.
    Omniauthable: adds Omniauth (https://github.com/intridea/omniauth) support;
    Confirmable: sends emails with confirmation instructions and verifies whether an account is already confirmed during sign in.
    Recoverable: resets the user password and sends reset instructions.
    Registerable: handles signing up users through a registration process, also allowing them to edit and destroy their account.
    Rememberable: manages generating and clearing a token for remembering the user from a saved cookie.
    Trackable: tracks sign in count, timestamps and IP address.
    Timeoutable: expires sessions that have no activity in a specified period of time.
    Validatable: provides validations of email and password. It's optional and can be customized, so you're able to define your own validations.
    Lockable: locks an account after a specified number of failed sign-in attempts. Can unlock via email or after a specified time period.

# Gemfile
gem 'devise'

# install generators and initializer (config/initializers/devise.rb - customize it!)
rails generate devise:install

# generate db migration and model
rails generate devise MODEL
