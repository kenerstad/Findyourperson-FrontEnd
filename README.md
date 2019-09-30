# person-frontend

## Instructions
A front end application that leverages backend REST api to create, modify & delete persons.
A person has 2 properties: Name & age.
Application uses vue routing to onepage views.

## Project setup
npm install
npm run serve

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### Installed plugins:
vue-resource, vue-router, eslint, babel.

### Views:
People view:
Components: PersonList, PersonWithSave
The people view relegates most if not all calls to backend 

People with save view:
Components: PersonListWithSave, Person

### Unresolved issues
[ GET http://192.168.42.20:8082/sockjs-node/info?t=1569799991163 net::ERR_CONNECTION_TIMED_OUT ] - Error message keeps popping up on
people & peoplewithsavefunction views: Possible CORS issue?
undo function on 

### planned features
* Convert lists to proper forms. (Considering using https://www.npmjs.com/package/vue-tables-2)

* Include https://vuelidate.netlify.com/#sub-basic-form for proper validation.

* Undo function for calls to backend. (Deletes, updates, etc..)

* Tests(Integration, unit, etc..)
