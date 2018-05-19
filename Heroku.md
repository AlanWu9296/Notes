# Deploy the app

1. `heroku create` in the project git folder to create the app on Heroku
2. `git push heroku master` now deploy the code on Heroku
3.  `heroku ps:scale web=1` to ensure that at least one instance of the app is running
4.  `heroku open` to open the app in the browser

# View logs

1. `heroku logs --tail`

# Define a Procfile

Use a [Procfile](https://devcenter.heroku.com/articles/procfile), a text file in the root directory of your application, to explicitly  declare what command should be executed to start your app.

`web: node index.js`

# Run the app locally

`heroku local web`

Just like Heroku, `heroku local` examines the `Procfile` to determine what to run.

Open <http://localhost:5000> with your web browser. You should see your app running locally.

To stop the app from running locally, in the CLI, press `Ctrl`+`C` to exit.