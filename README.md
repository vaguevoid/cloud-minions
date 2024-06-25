# Void GitHub Actions

This public repository contains GitHub actions for use with the [Void](https://void.dev) game development platform

The following high level composite actions are recommended

  * **Share on Steam** - Build, Package, and Deploy to Steam
  * **Share on Discord** - Build, Package, and Deploy to Discord
  * _...more to come_

## Share on Steam

This [action](share/on/steam/action.yml) can be used to build, package, and deploy your Void game to Steam

```yaml
  runs-on: ubuntu-latest
  steps:
    - name: Checkout Repo
      uses: actions/checkout@v4
    
    - name: Build, Package, and Deploy to Steam
      uses: vaguevoid/actions/share/on/steam@alpha
      with:
        executable:       "my-game"                       # name (without extension) to use for generated executables
        username:         gordonja                        # your Steam username
        configVdf:        ${{ secrets.STEAM_CONFIG_VDF }} # a Saved Steam login session (see below)
        appId:            "xxxx"                          # the Steam Application ID
        windowsDepotId:   "xxxx"                          # the Steam Depot ID for your Windows binaries
        macosDepotId:     "xxxx"                          # the Steam Depot ID for your MacOS binaries (optional)
        linuxDepotId:     "xxxx"                          # the Steam Depot ID for your Linux binaries (optional)
        setLiveBranch:    "beta"                          # the Steam branch to set live with this build (optional)
        buildDescription: "latest and greatest"           # a build description (optional)
```

### Steam Authorization File (config.vdf)

For actions that deploy to steam you will need to provide a `configVdf` configuration file generated
by the `steamcmd +login` action. To generate this file you can use the `vaguevoid/steam-login`
Docker image from your local development machine to generate a `config.vdf` file which you can then
copy into a `STEAM_CONFIG_VDF` GitHub actions secret. You may have to manually regenerate and
upload a new secret every 30 days.

From your terminal:

```bash
> docker run -it -v .:/out vaguevoid/steam-login [USERNAME]

Steam> Enter you password:
Steam> Enter your Steam Guard code:

> cat ./config.vdf    # copy the contents of this file to your `STEAM_CONFIG_VDF` GitHub secret
```

## Share on Discord

> coming soon

## Other Actions

The high level share actions above make many simplifying assumptions. If they do not meet your needs
you can drop down to the lower level actions below and combine them to build your
own workflow steps:

  * [Build with Vite](build/vite/readme.md) - Build for the web using Vite
  * [Package with Electron](package/electron/readme.md) - Package existing web build using Electron
  * [Deploy to Steam](deploy/steam/readme.md) - Deploy existing packaged game to Steam
  * _...more to come_
