# Gaston Jorquera

Source code for [gaston.life](https://gaston.life).

## Development

This site is built with [Hugo](https://gohugo.io). To run a local server, that
refreshes itself on file changes, run:

```bash
./script/server
```

### Main Directories

* `assets/`: CSS and Javascript files.
* `content/`: Content for the different content types.
* `layouts/`: HTML for the different content types.
* `static/`: Files copied without any transformations.

## Hosting

* Infrastructure hosted on [AWS](https://aws.amazon.com/).
* Pages hosted on [GitHub Pages](https://pages.github.com/).

### Infrastructure

The infrastructure that this site needs is defined in the
[infra.yml](script/infra.yml) file.

To provision the infrastructure, run:

```bash
./script/provision
```

### Deployment

The deployment is done using two repositories, based on [Hugo's
guidance](https://gohugo.io/hosting-and-deployment/hosting-on-github/).

* `gaston.life` (this repository): Contains the source to build the site.
* `gjorquera.github.io`: Contains the built, static site.

To deploy, run:

```bash
./script/deploy
```

## Credits

The idea for this repository was taken from [Ben Orenstein's Home
Page](https://github.com/r00k/r00k.github.io). The content was highly inspired
by [Derek Siver's Home Page](https://sivers.org).

Thanks.
