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
rails new rent_a_crowd
</pre>

- Add following line to Gemfile

<pre>
gem 'devise'
</pre>

- Install devise https://github.com/plataformatec/devise#getting-started
  - Add "gem 'devise'" to Gemfile

<pre>
bundle install
rails generate devise:install
</pre>

  - Add following line to config/environments/development.rb

<pre>
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
</pre>

  - 
