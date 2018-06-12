# To use Watir on production

This solution is working with the browser Chrome.

## Add the buildpack in your project

Add the following buildpacks in the settings tab in your heroku project.

* https://github.com/heroku/heroku-buildpack-chromedriver

* https://github.com/heroku/heroku-buildpack-google-chrome

## Add the following lines your rails project 

Add the following line in your .ENV file :

```
GOOGLE_CHROME_SHIM = '/app/.apt/usr/bin/google-chrome'
```

Add the following line before each Watir::Browser calls :

```
opts = {
    headless: true
  }

  if (chrome_bin = ENV.fetch('GOOGLE_CHROME_SHIM', nil))
    opts.merge!( options: {binary: chrome_bin})
  end 
```

And call your browser as following :

```
browser = Watir::Browser.new :chrome, opts
```
