# Jekyll website development made easy

<img
src="https://user-images.githubusercontent.com/875669/35621353-e78a6956-0638-11e8-8e07-3d96e9e91dd7.png"
height=48 width=72 alt="Docker Logo" /> <img
src="https://user-images.githubusercontent.com/875669/42405293-405f1cae-8148-11e8-8c81-c464be1d36bb.jpeg"
height=48 width=41 alt="Ruby Logo" /> <img
src="https://user-images.githubusercontent.com/875669/42405359-638eff9a-8149-11e8-8812-fa950f6a0387.png"
height=48 width=104 alt="Jekyll Logo" />

Learn more [about Jekyll][jekyll].  This project aims to help getting started
with Jekyll easy by using [Docker][docker] for managing Jekyll dependencies.

This project is a development environment for Jekyll which solely relies on
Docker provide all dependencies for Jekyll.

# Features

- A self contained development environment based on the lightweight Alpine
  Linux.
- Uses only [Trusted Official Repositories][official-repos].
- Examples rely on [bundler][bundler] for ruby best practices in package
  management.

# Requirements

* [Docker][docker]

Optionally, `make` makes it easy.  Otherwise, if you do not wish to use `make`
then reference the [`Makefile`](Makefile) for running commands.

# Getting started (first time)

1. Fork this repository.
2. Execute `make` to start an initial docker image with a minimal jekyll
   installation.
3. Initialize your first Jekyll website inside of the docker container and exit.

   ```
   jekyll new --force .
   rm Gemfile.lock
   # recreate the Gemfile.lock
   bundle install
   exit
   ```

4. Now that you're outside of the container commit your initial changes.

   ```
   git add -A
   git commit -m "My first jekyll site"
   ```

5. Rebuild your docker container so that it properly includes your development
   dependencies.

   ```
   make build
   ```

Now you're ready to develop your website.  Proceed to the _Development Guide_
section.

# Development Guide

Start the interactive development environment.  This will create a shell inside
of a docker container.  The container will share the host networking.

    make

From here, you're developing inside of the container.  Your working directory is
the root of your git repository so that you can develop.  Start the web server
to render your jekyll website.

    jekyll serve

Visit `http://127.0.0.1:4000/` in your web browser to see the website.

> Pro tip: skip the interactive environment by running `docker-compose up`.

If you add ruby packages to your `Gemfile` then you'll want to rebuild your
development container and update `Gemfile.lock`.  This must be done from within
the interactive environment.  The following commands are relevant for achieving
this.

```bash
# add your changes to Gemfile
# enter the interactive docker environment
make

# update Gemfile.lock
bundle install

# exit the docekr environment
exit

# rebuild the docker container to include the new Gemfile changes
make build

# restart the development environment which will now include new dependencies
# pre-installed.
make
```

When you're done developing you may wish to delete the development environment.
To delete, run the following.

    make clean

# Is it Magic?

No!  The `make` command reads and runs the [`Makefile`](Makefile).  The
`Makefile` includes several targets: `interactive`, `build`, `clean`, `serve`.
You can run any of the targets by running `make [TARGET]`.  If no `TARGET` is
specified then `make` will default to the first target defined in the
`Makefile`.  The following two commands will result in the same action.

```bash
# Run make with the default target: interactive
make

# Specify the interactive target
make interactive
```

which in turn
runs the `docker-compose` command.  The `docker-compose` command reads and runs
[`docker-compose.yml`](docker-compose.yml) which is using the [Docker Compose
File Version 2][compose-v2].

See also:

    docker-compose --help
    docker-compose run --help
    docker-compose down --help

[bundler]: https://bundler.io/
[compose-v2]: https://docs.docker.com/compose/compose-file/compose-file-v2/
[docker]: https://www.docker.com/community-edition
[jekyll]: https://jekyllrb.com/
[official-repos]: https://docs.docker.com/docker-hub/official_repos/
