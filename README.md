# DISCLAIMER

This is an Un-Official branch of SauceConnectPlugin used to build Vaadin Platform.

We needed to fork it because of the following reasons

- They are not publishing the plugin with new changes
- The plugin is not compatible with modern gradle versions

The artifact has been renamed to `com.vaadin.plugin.sauce:Gradle-Sauce-Connect-Plugin`

## Building

- To compile and install the plugin locally just run `./gradlew publishToMavenLocal`
- To deploy to gradle central, you need an account as described in this [article](https://plugins.gradle.org/docs/submit), then you can execute `./gradlew publishPlugins`


# Gradle SauceConnect Plugin

This is SauceConnect plugin that downloads, starts and stops SauceLab's SauceConnect application.

# Requirements

  - SauceLabs Account
  - Gradle initialized project

# Publishing

For now the following will publish the plugins:

```
    ./gradlew build publishPlugins
```

# Using the plugin

```
plugins {
    id "com.saucelabs.SauceConnectPlugin" version "0.0.14"
}

sauceconnect {
  username = "$System.env.SAUCE_USERNAME"
  key = "$System.env.SAUCE_ACCESS_KEY"
  options = "-v"
}

def sauce = [
    username: sauceconnect.username,
    key     : sauceconnect.key,
]

task "sauceTest"(dependsOn: startSauceConnect, type: Test) {

    outputs.upToDateWhen { false }  // Always run tests

    // Run in parallel depending on how good your machine is
    maxParallelForks Math.max(2, Runtime.runtime.availableProcessors().intdiv(2))

    reports {
        html.destination = reporting.file("$name/tests")
        junitXml.destination = file("$buildDir/test-results/$name")
    }

    systemProperty "sauce.username", sauce.username
    systemProperty "sauce.key", sauce.key
    systemProperty "testDriver", "sauce"
    systemProperty "geb.build.reportsDir", reporting.file("$name/geb")

}

// This is to make sure that SauceConnect is shutdown
sauceTest.finalizedBy stopSauceConnect

```

For a full working example that uses Geb-Spock test stack feel free to browse [Gradle-Sauce-Connect-Plugin-Example](https://github.com/aginteractive/Gradle-Sauce-Connect-Plugin-Example).

## Contributing

Contributions are welcome. Please see [CONTRIBUTING.md](CONTRIBUTING.md) for
details.
