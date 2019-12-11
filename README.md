# Gaston Jorquera

Code for [gaston.life](https://gaston.life).

## Development

This site is built with [Hugo](https://gohugo.io). To run a local server, that
refreshes itself on file changes, run:

```bash
./script/run
```

## Deployment

The deployment is done using two repositories based on
[Hugo's guidance](https://gohugo.io/hosting-and-deployment/hosting-on-github/).

* `gaston.life` (this repo): Contains the source to build the site.
* `gjorquera.github.io`: Contains the built, static site.

To deploy, run:

```bash
./script/deploy
```

### Main Directories

* `content/`: Contains the different content types.
* `layouts/`: Contains the HTML for the different content types.

## Credits

The idea for this repository was taken from [Ben Orenstein's Home Page](https://github.com/r00k/r00k.github.io).
The content was highly inspired by [Derek Siver's Home Page](https://sivers.org).

Thanks.
