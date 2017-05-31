# walkleys-scholar-syd-01-project

## (Week 2) User Stories
### Key user stories
### Data to store
- A participant can register their name, age, ethnicity, gender, interests, occupation, availability, location (state, suburb, postcode), contact details and photo so they can be identified by a client.
- A client can search for participants according to their criteria to find available and eligible participants for their event.
- A client can create an event by storing:
    - the number of participants
    - date and time of the event
    - location
    - what the participants would have to do
    - participant demographics (like asian women between 30 to 40)
    - where the event will be published (like syndicated via print, web, social media etc)
    - participant compensation
#### Key functions (analytics)
- An admin user can view the number of participants and clients.
- An admin user can view aggregate information about participants and clients, such as average age of participants to report demographics to potential clients.
- An administration users should be able to identify serial (eg multiple events) participants.
#### Key functions (invite participants)
- A client can send an invitation to eligible participants to their event.
- Participants can choose to only be invited to events with monetary compensation.
#### Key functions (event details)
- A client can make events publicly viewable, or viewable only to invited participants.
- A participant can see the number of events they could be invited to if they provided further information in their profile.
- Anonymous users can browse publicly available events.
- Anonymous users can create participant profiles.
#### Key functions (billing and payments)
- A client uses credits per successful participant invited to the event.
- Clients can offer compensation to participants for their time.
- A client can purchase credits by credit card payment.
- A client can provide feedback on participants attending events
#### Key functions (reviews)
- A participant can provide feedback on client events.
#### Key functions (social media)
- A participant can login to the service with their social media login, so they do not have to enter their personal details
#### Key functions (notifications)
- A participant will receive a notification when invited to an event
- A participant can select how they receive event invitations (email, SMS etc.)

### Deferred user stories
- A client can optionally invite participants to multiple events the client organises, up to a predefined frequency.
- A participant should be able to delete an account, with a reason.
#### Key functions (bring a friend)
- A client may allow a participant to bring a friend to an event.
- A participant can invite a friend to register as a participant.
#### Key functions (blacklisting)
- A participant can be blacklisted if they do not attend multiple events.
- A participant can be blacklisted if a client reports a participant for bad behaviour.

## (Week 3) Models, Generators and Scaffolds
- The instructions we used to build the generators and scaffolds

<pre>
rails new mustr
</pre>

- Create a landing page, and a dashboard page

<pre>
rails generate controller home index
rails generate controller dashboard index
</pre>

- Add following line to Gemfile

<pre>
# Using devise for login
gem 'devise'
</pre>

- Install devise https://github.com/plataformatec/devise#getting-started
  - Setup devise

<pre>
bundle install
rails generate devise:install
</pre>

  - Add following line to config/environments/development.rb

<pre>
# setup mailer
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
</pre>

  - Add a route to the home page created in "config/routes.rb"

  <pre>
  root to: "home#index"
  </pre>

  - Add messages to the app/views/layouts/application.html.erb

  <pre>
    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>
  </pre>

  - Generate Devise views

  <pre>
  rails g devise:views
  </pre>

  - Setup Devise user Model

  <pre>
  rails generate devise User
  rake db:migrate
  </pre>

  - Setup user models
    - Run following commands:
      <pre>
        rails generate model Admin
        rails generate model Client
        rails generate model Participant
      </pre>
    - Change the models to inherit from users, like in app/models/admin.rb
      <pre>
      class Admin < User
      end
      </pre>

      - Add following line to app/controllers/dashboard_controller.rb
      <pre>
        before_action :authenticate_user!
      </pre>

      <pre>
      class DashboardController < ApplicationController
        before_action :authenticate_user!
        def index
        end
      end
      </pre>

      - Set Devise scoped views to true so can have different views for users (in config/initializers/devise.rb)

      config.scoped_views = true

      - In config/routes.rb, change the devise_for from :users to other user types

      devise_for :admins
      devise_for :participants
      devise_for :clients

      - Add logout button to application.html.erb file

      <pre>
      <% if !admin_signed_in? %>
          <%= link_to 'Admin log in', new_admin_session_path %>
      <% end %>
      <% if !participant_signed_in? %>
          <%= link_to 'Participant log in', new_participant_session_path %>
      <% end %>
      <% if !client_signed_in? %>
          <%= link_to 'Client log in', new_client_session_path %>
      <% end %>
      <% if admin_signed_in? %>
          <%= link_to 'Admin log out', destroy_admin_session_path, method: :delete %>
      <% end %>
      <% if client_signed_in? %>
          <%= link_to 'Client log out', destroy_client_session_path, method: :delete %>
      <% end %>
      <% if participant_signed_in? %>
          <%= link_to 'Participant log out', destroy_participant_session_path, method: :delete %>
      <% end %>
      </pre>

      - Add devise group to application controller so we can use current_user and user_signed_in? functions
      <pre>
      devise_group :user, contains: [:admin, :client, :participant]
      </pre>

      - Add fields to user models

      <pre>
      rails generate migration add_credit_balance_to_clients credit_balance:int
      rails generate migration add_credit_rate_to_clients credit_rate:decimal

      rails generate migration add_name_to_participants name:string
      rails generate migration add_ethnicity_to_participants ethnicity:string
      rails generate migration add_gender_to_participants gender:string
      rake db:migrate
      </pre>

  - Setup other classes
      rails generate scaffold Event name:string num_participants:integer start_time:datetime finish_time:datetime address:text instructions:text demographic:text publication_details:text monetary_compensation:decimal other_compensation:text publicly_viewable:boolean client:references

      rails generate scaffold Invitation event:references participant:references accepted:boolean

   - for each "references" in scaffold, change the last parameter in each f.collection_select to some meaningful parameter (in this example, it's changing a reference to a user who owns multiple accounts in a system.)

     - Replace form control for selecting users in accounts (/app/views/-insert-model-name-/_form.html.erb)
        <%= f.collection_select(:user_id, User.all, :id, :email ) %>

     - Replace form control for displaying users in accounts (/app/views/-insert-model-name-/show.html.erb and /app/views/accounts/index.html.erb )
         <dd><%= @account.user.email %></dd>
