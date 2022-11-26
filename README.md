<div align="center">

# my Rails7 Devise JWT Authentication API | Template with Tutorial

## INSERT BADGES HERE

</div>

<br>
<br>


<details>
<summary>
PARTIE 1 : COMMENT UTILISER CE TEMPLATE
</summary>
<br>
<br>

1)Clone this repo;

2)Delete 'config/credentials.yml.enc' file in the cloned repository;

3)Open API backend folder in a Terminal window, and run the following commands:

```
rails secret
```

4)Copy the string (secret key) that you get in your notepad or whatever (you'll have to paste it later).

```
EDITOR=nano rails credentials:edit
```
(will open a file in Nano editor)


5)Add at the bottom of the file opened with Nano:
```
devise:
  jwt_secret_key: [copied secret key] // ⚠ there are 2 spaces at the beginning of this line
```



```
bundle install
rails db:create db:migrate
rails server
```

Your API should be running on port localhost:3000  
You can test is folowing the below instructions -> _to be translated someday..._

### Registration (Sign Up)

`POST /users`

Données attendues :

```json
{
	"user": {
		"email": string,
		"password": string
	}
}
```

Pour la tester :

```sh
curl -XPOST -H "Content-Type: application/json" -d '{ "user": { "email": "test@example.com", "password": "12345678" } }' http://localhost:3000/users
```

Réponse :

```sh
=> {"message":"Signed up successfully.","user":{"id":[id],"email":"test@example.com","created_at":[timestamp],"updated_at":[timestamp]}
```

### Login

`POST /users/sign_in`

Données attendues

```json
{
	"user": {
		"email": string,
		"password": string
	}
}
```

Pour la tester :

```sh
curl -XPOST -i -H "Content-Type: application/json" -d '{ "user": { "email": "test@example.com", "password": "12345678" } }' http://localhost:3000/users/sign_in
```

Réponse :

```sh
HTTP/1.1 200 OK
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 0
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Permitted-Cross-Domain-Policies: none
Referrer-Policy: strict-origin-when-cross-origin
Content-Type: application/json; charset=utf-8
Vary: Accept, Origin
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyMDQiLCJzY3AiOiJ1c2VyIiwiYXVkIjpudWxsLCJpYXQiOjE2NDYyMTk4MTEsImV4cCI6MTY0NjIyMzQxMSwianRpIjoiZWMxNDk3NWItOTNkYS00YTE1LTg1YTQtZmQ0ODllOTI2MTIwIn0.ZxRTdqSQ-Ahh4To9qdheeMewFHmbZtvWa_gSYx5mD38
Set-Cookie: _interslice_session=vOm61TiX5r758FI7DXxo07gRo%2F1lB08%2BrjKnf5N2q5oIOA4P3CI943u%2FbLSS3lJCyu%2FrFmLF8%2FliLCxhQTZN4DqNGgGgjZh6koGGyCxdFwshloUmSByg0D8vRA21kEQcCguvQ8BwJ1alzn6N9fAjXussdx63iL87TSUGhuWgSv3Ze4BkD1WsRG%2FFlH%2BJ%2Ba4mraPkGZCiQmfBlRLDjZ7n4mmWaE1ASsAhXmhf%2BeC79ag%2BQgE3ZOHkTzRUmnQft4BGeVC51ITCfvW47Cbi8elBQsfs2IzROxe9qtDOklzDcA%3D%3D--U%2FLRbl1%2FWXHqxKhR--lcsdl17IGM7jOT14NN8qZg%3D%3D; path=/; HttpOnly; SameSite=Lax
ETag: W/"3f408df0bede3cd5797e2190eefd79d9"
Cache-Control: max-age=0, private, must-revalidate
X-Request-Id: f1e51158-e4c6-42f2-bb94-535869cdccb5
X-Runtime: 0.256978
Server-Timing: start_processing.action_controller;dur=0.2275390625, sql.active_record;dur=1.86376953125, instantiation.active_record;dur=0.0888671875, process_action.action_controller;dur=234.275390625
Transfer-Encoding: chunked

{"message":"You are logged in.","user":{"id":204,"email":"test@example.com","created_at":"2022-03-01T19:50:54.482Z","updated_at":"2022-03-01T19:50:54.482Z"}}
```

<br>

### Login with token

`GET /member-data`

Authentification nécessaire

Pour la tester :

```sh
curl -XGET -H [le token qui était dans Authorization dans la requête de login] -H "Content-Type: application/json" http://localhost:3000/member-data
```

par exemple ça donne ça :
```
curl -XGET -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwic2NwIjoidXNlciIsImF1ZCI6bnVsbCwiaWF0IjoxNjYzMjg5OTczLCJleHAiOjE2NjMyOTM1NzMsImp0aSI6IjhmYTEwY2E5LWJjNWItNDc0Zi04ZGEwLTU0MjFiYTM1YzJjMCJ9.WEHytPD0hRKujGCRh_MxrcZ93jSZ5UqNT_Dp-C6J__I" -H "Content-Type: application/json" http://localhost:3001/member-data
```

Réponse :

```sh
{"message":"If you see this, you're in!","user":{"id":204,"email":"test@example.com","created_at":"2022-03-01T19:50:54.482Z","updated_at":"2022-03-01T19:50:54.482Z"}}
```

<br>

### Logout

`DELETE /users/sign_out`

Authentification nécessaire

Pour la tester :

```sh
curl -XDELETE -H "Authorization: [le token qui était dans Authorization dans la requête juste avant]" -H "Content-Type: application/json" http://localhost:3000/users/sign_out
```

Réponse :

```sh
{"message":"You are logged out."}
```
</details>


<details>
<summary>
PARTIE 2 : COMMENT REPRODUIRE CETTE API (step-by-step)
</summary>
<br>
<br>

# 1. Création de l’API 🛤

### Création d’une app Rails en mode API _(Backend only, la partie Frontend fera l'objet d'une autre Appli à relier à celle-ci)  
=> `rails new my_api --api --postgresql`

<br>

### Rajout de 3 gems : devise, devise-jwt, et rack-cors

=> `bundle add devise devise-jwt rack-cors`

- [Devise](https://github.com/heartcombo/devise) sert au setup de tout le système d’authentification en tant que tel
- [Devise-jwt](https://github.com/waiting-for-dev/devise-jwt) est une extension de Devise permettant d’utiliser les JWT token pour l’authentification
- [Rack CORS](https://github.com/cyu/rack-cors) permet de faire des requêtes cross-domains _(en gros de pouvoir faire des requêtes à l'API depuis un autre domaine)_

<br>

### Configuration de Rack CORS

C’est parti pour quelques modifications dans le fichier `config/initializers/cors.rb`

Ces changements permettent d’autoriser n’importe quel site à faire des requêtes à l’API, pour autoriser une seule origine ➡️ `origins "[url]"`

```ruby
# config/initializers/cors.rb

# Be sure to restart your server when you modify this file.

# Avoid CORS issues when API is called from the frontend app.
# Handle Cross-Origin Resource Sharing (CORS) in order to accept cross-origin AJAX requests.

# Read more: https://github.com/cyu/rack-cors

Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'

    resource '*',
             headers: :any,
             methods: %i[get post put patch delete options head],
             expose: %w[Authorization Uid]
  end
end
```

<br>

### Installation de Devise et génération de la table User

=> `rails g devise:install`

=> `rails g devise User`

<br>
<br>

# Devise JWT 💲

### Génération de la Denylist

La DenyList est une méthode révocation de Token JWT, en gros à chaque fois qu'un utilisateur se déconnecte ou que le token est expiré un nouveau token sera généré pour cet utilisateur

=> `rails g model jwt_denylist jti:string exp:datetime`

- Le jti est l’identifiant unique d’un token
- Exp contient sa date d’expiration

> ⚠️ Pour que tout fonctionne, vous devez renommer le fichier de migration (de `[timestamp]_create_jwt_denylists.rb` à `[timestamp]_create_jwt_denylist.rb`), la classe et la table au singulier (voir en-dessous)

Fichier de migration :

```ruby
# db/migrate/20220228223034_create_jwt_denylist.rb

class CreateJwtDenylist < ActiveRecord::Migration[7.0]
  def change
    create_table :jwt_denylist do |t|
      t.string :jti, null: false
      t.datetime :exp, null: false

      t.timestamps
    end
    add_index :jwt_denylist, :jti
  end
end
```

<br>

### Petit changements au modèle User

- `:jwt_authenticatable` permet de dire à Devise que `User` utilise jwt pour l’authentification
- `:jwt_revocation_strategy` permet de dire à `User` comment il doit révoquer les tokens, et qu’il doit utiliser le modèle `JwtDenylist` pour ça

```ruby
# app/models/user.rb

class User < ApplicationRecord
	# Il faut ajouter les deux modules commençant par jwt
	devise :database_authenticatable, :registerable,
	:jwt_authenticatable,
	jwt_revocation_strategy: JwtDenylist
end
```

### Petits ajouts des familles au Model `JwtDenylist`

On indique aussi au modèle `JwtDenylist` qu’il doit utiliser la stratégie de révocation `denylist` (oui oui)

```ruby
# app/models/jwt_denylist.rb

class JwtDenylist < ApplicationRecord
  include Devise::JWT::RevocationStrategies::Denylist

  self.table_name = 'jwt_denylist'
end
```

<br>

### `rails db:migrate` 🙂

<br>
<br>

# Devise API JWT Controllers for Sessions and Registrations 🧒

### Créer le fichier `members_controller.rb`

La méthode `show` permettra de s’authentifier avec un token au lieu d’avec l’email et le password

```ruby
# app/controllers/members_controller.rb

class MembersController < ApplicationController
  before_action :authenticate_user!

  def show
    user = get_user_from_token
    render json: {
      message: "If you see this, you're in!",
      user: user
    }
  end

  private

  def get_user_from_token
    jwt_payload = JWT.decode(request.headers['Authorization'].split(' ')[1],
                             Rails.application.credentials.devise[:jwt_secret_key]).first
    user_id = jwt_payload['sub']
    User.find(user_id.to_s)
  end
end
```

<br>

### Création des Users

Deux nouveaux controllers à créer, qui modifieront les controllers de registration et de session de Devise

> ⚠️ Il faut créer ces fichiers dans un dossier `users` dans `app/controllers` (voir le commentaire en haut des snippets)

```ruby
# app/controllers/users/registrations_controller.rb

class Users::RegistrationsController < Devise::RegistrationsController
  respond_to :json

  private

  def respond_with(resource, _opts = {})
    register_success && return if resource.persisted?

    register_failed
  end

  def register_success
    render json: {
      message: 'Signed up sucessfully.',
      user: current_user
    }, status: :ok
  end

  def register_failed
    render json: { message: 'Something went wrong.' }, status: :unprocessable_entity
  end
end
```

```ruby
# app/controllers/users/sessions_controller.rb

class Users::SessionsController < Devise::SessionsController
  respond_to :json

  private

  def respond_with(_resource, _opts = {})
    render json: {
      message: 'You are logged in.',
      user: current_user
    }, status: :ok
  end

  def respond_to_on_destroy
    log_out_success && return if current_user

    log_out_failure
  end

  def log_out_success
    render json: { message: 'You are logged out.' }, status: :ok
  end

  def log_out_failure
    render json: { message: 'Hmm nothing happened.' }, status: :unauthorized
  end
end
```

<br>
<br>

# Devise JWT Secret Key 🔑

### Config du secret JWT utilisé pour décoder les tokens dans `config/initializers/devise.rb`

```ruby
Devise.setup do |config|
	# Plein de code
	config.jwt do |jwt|
		jwt.secret = Rails.application.credentials.devise[:jwt_secret_key]
	end
	# Encore tout plein de code
end
```

1. Génération du secret

   - `rake secret`
   - Copie de la string générée
   - `EDITOR=nano rails credentials:edit`
   - Ajout en bas du fichier de :

   ```bash
   devise:
     jwt_secret_key: [clé copiée] // ⚠ Il faut mettre 2 espaces au début de cette ligne
   ```

<br>
<br>

# Routes 🛣

### Go `config/routes.rb`

```ruby
# config/routes.rb

Rails.application.routes.draw do
  devise_for :users,
             controllers: {
               sessions: 'users/sessions',
               registrations: 'users/registrations'
             }
  get '/member-data', to: 'members#show'
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"
end
```

<br>
<br>

# Configure Session Store in Rails 7

1. Config pour utiliser les cookies dans `config/application.rb`

```ruby
# config/application.rb

module DeviseVue
  class Application < Rails::Application
    # Du code cool

    # This also configures session_options for use below
    config.session_store :cookie_store, key: '_interslice_session'

    # Required for all session management (regardless of session_store)
    config.middleware.use ActionDispatch::Cookies

    config.middleware.use config.session_store, config.session_options

    # Plein de code
  end
end
```

Et voilà ! 🎉

<br>
<br>

# Et les routes c’est quoi ?

### Register

`POST /users`

Données attendues :

```json
{
	"user": {
		"email": string,
		"password": string
	}
}
```

Pour la tester :

```sh
curl -XPOST -H "Content-Type: application/json" -d '{ "user": { "email": "test@example.com", "password": "12345678" } }' http://localhost:3000/users
```

Réponse :

```sh
=> {"message":"Signed up successfully.","user":{"id":[id],"email":"test@example.com","created_at":[timestamp],"updated_at":[timestamp]}
```

### Login

`POST /users/sign_in`

Données attendues

```json
{
	"user": {
		"email": string,
		"password": string
	}
}
```

Pour la tester :

```sh
curl -XPOST -i -H "Content-Type: application/json" -d '{ "user": { "email": "test@example.com", "password": "12345678" } }' http://localhost:3000/users/sign_in
```

Réponse :

```sh
HTTP/1.1 200 OK
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 0
X-Content-Type-Options: nosniff
X-Download-Options: noopen
X-Permitted-Cross-Domain-Policies: none
Referrer-Policy: strict-origin-when-cross-origin
Content-Type: application/json; charset=utf-8
Vary: Accept, Origin
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyMDQiLCJzY3AiOiJ1c2VyIiwiYXVkIjpudWxsLCJpYXQiOjE2NDYyMTk4MTEsImV4cCI6MTY0NjIyMzQxMSwianRpIjoiZWMxNDk3NWItOTNkYS00YTE1LTg1YTQtZmQ0ODllOTI2MTIwIn0.ZxRTdqSQ-Ahh4To9qdheeMewFHmbZtvWa_gSYx5mD38
Set-Cookie: _interslice_session=vOm61TiX5r758FI7DXxo07gRo%2F1lB08%2BrjKnf5N2q5oIOA4P3CI943u%2FbLSS3lJCyu%2FrFmLF8%2FliLCxhQTZN4DqNGgGgjZh6koGGyCxdFwshloUmSByg0D8vRA21kEQcCguvQ8BwJ1alzn6N9fAjXussdx63iL87TSUGhuWgSv3Ze4BkD1WsRG%2FFlH%2BJ%2Ba4mraPkGZCiQmfBlRLDjZ7n4mmWaE1ASsAhXmhf%2BeC79ag%2BQgE3ZOHkTzRUmnQft4BGeVC51ITCfvW47Cbi8elBQsfs2IzROxe9qtDOklzDcA%3D%3D--U%2FLRbl1%2FWXHqxKhR--lcsdl17IGM7jOT14NN8qZg%3D%3D; path=/; HttpOnly; SameSite=Lax
ETag: W/"3f408df0bede3cd5797e2190eefd79d9"
Cache-Control: max-age=0, private, must-revalidate
X-Request-Id: f1e51158-e4c6-42f2-bb94-535869cdccb5
X-Runtime: 0.256978
Server-Timing: start_processing.action_controller;dur=0.2275390625, sql.active_record;dur=1.86376953125, instantiation.active_record;dur=0.0888671875, process_action.action_controller;dur=234.275390625
Transfer-Encoding: chunked

{"message":"You are logged in.","user":{"id":204,"email":"test@example.com","created_at":"2022-03-01T19:50:54.482Z","updated_at":"2022-03-01T19:50:54.482Z"}}
```

<br>

### Login with token

`GET /member-data`

Authentification nécessaire

Pour la tester :

```sh
curl -XGET -H [le token qui était dans Authorization dans la requête de login] -H "Content-Type: application/json" http://localhost:3000/member-data
```

par exemple ça donne ça :
```
curl -XGET -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwic2NwIjoidXNlciIsImF1ZCI6bnVsbCwiaWF0IjoxNjYzMjg5OTczLCJleHAiOjE2NjMyOTM1NzMsImp0aSI6IjhmYTEwY2E5LWJjNWItNDc0Zi04ZGEwLTU0MjFiYTM1YzJjMCJ9.WEHytPD0hRKujGCRh_MxrcZ93jSZ5UqNT_Dp-C6J__I" -H "Content-Type: application/json" http://localhost:3001/member-data
```

Réponse :

```sh
{"message":"If you see this, you're in!","user":{"id":204,"email":"test@example.com","created_at":"2022-03-01T19:50:54.482Z","updated_at":"2022-03-01T19:50:54.482Z"}}
```

<br>

### Logout

`DELETE /users/sign_out`

Authentification nécessaire

Pour la tester :

```sh
curl -XDELETE -H "Authorization: [le token qui était dans Authorization dans la requête juste avant]" -H "Content-Type: application/json" http://localhost:3000/users/sign_out
```

Réponse :

```sh
{"message":"You are logged out."}
```
</details>



<details>
<summary>
Tuto vidéo similaire : Devise API Authentication With Vue JS | Ruby on Rails 7 Tutorial by Deanin
</summary>
<br>
<br>

# 🎥 Vidéo Tuto similaire avec Front en VueJS : 

[![Lien de la vidéo de base](https://i.ytimg.com/vi/PqizV5l1yFE/maxresdefault.jpg)](https://www.youtube.com/watch?v=PqizV5l1yFE)
</details>


























