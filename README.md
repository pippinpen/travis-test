# Setting up Travis & Heroku for CI

1. Make sure you have installed both the travis (https://github.com/travis-ci/travis.rb#installation) and heroku (https://devcenter.heroku.com/articles/heroku-cli#download-and-install) CLI tools.

2. Login with heroku and travis. You should be on .com by default with travis but you may need to add the flag (`--pro` or `--com`)

## The project

1. `npm init -y`
2. `npm i express && npm i -D jest supertest`
3. Change the test script in package.json to `"test": "jest",` and add some config to the file
```json
"jest": {
    "testEnvironment": "node",
    "coveragePathIgnorePatterns": [
      "/node_modules/"
    ]
  }
```
4. Add a start script: `"start": "node index.js"
5. Create a simple node server (e.g. `index.js` and `app.js`)
6. Create a test file (e.g. `tests/routes.test.js`)
7. Add test code to make sure the server works
8. run `npm run test` and `npm start` to ensure both work
9. `git init` (add a `.gitignore`)
9. Make a dev branch too
10. Push both dev and main to github
11. Switch to dev

## Travis integration

1. Go sign up at travis.com with github
2. Link your repository to travis
3. add your `.travis.yml` to the project with the following code:

```YAML
language: node_js
node_js:
  - 10.13.0
install:
  - npm install
script:
  - npm run test
branches:
  only:
    - main
```

On push you should see the tests run in your travis dashboard. Now to add the push to heroku:

## Heroku integration

1. You need a heroku API key that you can generate under your profile page
2. You need to put it as an environment variable in your travis.yml like so:

```YAML
language: node_js
node_js:
- 10.13.0
install:
- npm install
script:
- npm run test
branches:
  only:
  - main
  - dev
deploy:
  provider: heroku
  api_key: $HEROKU_API_KEY
  app:
    main: travis-demo-prod
    dev: travis-demo-staging

```

3. You need to put in the Travis dashboard as an environment variable (You don't need the `$` in the name). Paste the heroku api key as the value

4. Create 2 apps on heroku, one for stagin and one for live and put the app name against the branch name, as I have done in the last 2 lines of the YAML above

5. Push your new .travis.yml file to github and it should deploy to heroku (on the staging app)

6. Check out main and merge from dev and push - you should now see a build and deploy to the production app on heroku


