# Neverbland Junior DevOps Challenge
## The Task
The goal is simple. To get this app up and running in a prod environment

As such, we'd like you to fork this repo and create a GitHub Action workflow that

:stopwatch: Will run upon a push to main (but can also be manually triggered)

:building_construction: Will build the project 

:rocket: Will deploy the build to an online (production) environment.

AWS would be the destination of choice (we'll let you choose which AWS service might be best though). If you don't want to use AWS, no problem. Pick something else, but please explain your choice.

Please document your work and choices. You can do this inline, in the README or both. When you're finished, send us a link to your repo.

## How to use

Jump into the Repo directory

`cd blog-starter-ts-devops`

Inside that directory, you can run several commands:


Install the dependencies

`npm install`

 Starts the development server.

`npm run dev`

Builds the app for production.

`npm run build`

 Runs the built app in production mode.

`npm run start`


## Notes

This repo uses the Next.js example [blog-starter-typescript](https://github.com/vercel/next.js/tree/canary/examples/blog-starter-typescript) project.

This blog-starter-typescript uses [Tailwind CSS](https://tailwindcss.com). To control the generated stylesheet's filesize, this example uses Tailwind CSS' v2.0 [`purge` option](https://tailwindcss.com/docs/controlling-file-size/#removing-unused-css) to remove unused CSS.

[Tailwind CSS v2.0 no longer supports Node.js 8 or 10](https://tailwindcss.com/docs/upgrading-to-v2#upgrade-to-node-js-12-13-or-higher). To build your CSS you'll need to ensure you are running Node.js 12.13.0 or higher in both your local and CI environments.
