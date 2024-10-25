# Matrix collection for Bruno

## Setup

1. Clone the GitHub repo
    ```bash
    git clone https://github.com/Twi1ightSparkle/matrix-bruno.git
    ```
1. Install [Bruno](https://www.usebruno.com/)
1. Copy `.env.sample` to `Matrix/.env`
1. Copy `environments.sample` to `Matrix/environments`. Alternatively, if you wish to have your environments
    under source control, symlink your collection to `Matrix/environments`
1. For every environment you want to connect to, make a copy of `Matrix/environments/_Template.bru` and add your secrets
    environment variables in `Matrix/.env`
1. In Bruno, click `Open collection`
1. Choose the `Matrix` directory in the cloned repo

## Tests/Visualizations

Bruno's support for [visualizations](https://learning.postman.com/docs/sending-requests/response-data/visualizer/) is
currently only available in Gold Edition
([source](https://github.com/usebruno/bruno/issues/2633#issuecomment-2408801159)), the visualization code from Postman
is commented out for now.
