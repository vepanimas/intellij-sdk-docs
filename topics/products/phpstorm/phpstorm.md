[//]: # (title: PhpStorm Plugin Development)

<!-- Copyright 2000-2022 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

[PhpStorm](https://www.jetbrains.com/phpstorm/) is an IntelliJ Platform-based product.
This page describes configuring plugin projects targeting PhpStorm.

See also:
* [](plugin_alternatives.md#phpstorm-advanced-metadata)
* [](php_open_api.md)
* [](existing_plugins.md)

> Please join the dedicated [intellij-php](https://jetbrains-platform.slack.com/archives/C5P9YB0LT/p1653913208725609) Slack channel to discuss PHP related plugin development.
>
{type="tip"}

## Configuring Plugin Projects Targeting PhpStorm


<tabs>

<tab title="PhpStorm IDE">

The configuration of targeting PhpStorm IDE follows the methods described in [Configuring Plugin Projects Using a Product-Specific Attribute](dev_alternate_products.md#configuring-plugin-projects-using-a-product-specific-attribute).

The possibility of configuring the `PS` IntelliJ Platform type was introduced in [](tools_gradle_intellij_plugin.md) 1.8.0.
Only the versions of PhpStorm 2022.2 and newer are supported.

The table below summarizes the Gradle IntelliJ Plugin attributes to set in the plugin project's Gradle build script.
Click on an entry in the table's *Attribute* column to go to the documentation about that attribute.

| `gradle-intellij-plugin` Attribute                                               | Attribute Value                                                                |
|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| [`intellij.type`](tools_gradle_intellij_plugin.md#intellij-extension-type)       | `PS` for PhpStorm.                                                             |
| [`intellij.version`](tools_gradle_intellij_plugin.md#intellij-extension-version) | Set to the targeted `PS` version (only versions 2022.2+ are supported).        |

</tab>

<tab title="PHP Plugin">

PhpStorm plugins targeting versions older than 2022.2 are developed using the Ultimate Edition of IntelliJ IDEA.
The IntelliJ IDEA Ultimate Edition (with the PHP plugin) must be used for developing PhpStorm plugins because the PHP plugin is incompatible with IntelliJ IDEA Community Edition.
However, this IntelliJ IDEA Ultimate configuration runs the risk of accidentally using some APIs that are not available in PhpStorm.
The recommended best practice is to use PhpStorm for testing.

Configuration of a Gradle-based PhpStorm plugin project is used as a tutorial in the section [Configuring Plugin Projects using the IntelliJ IDEA Product Attribute](dev_alternate_products.md#configuring-plugin-projects-using-the-intellij-idea-product-attribute).
Many techniques are discussed, such as choosing a version of IntelliJ IDEA Ultimate given a targeted version of PhpStorm.

The table below summarizes the [](tools_gradle_intellij_plugin.md) attributes to set in the plugin project's Gradle build script.
Click on an entry in the table's *Attribute* column to go to the documentation about that attribute.
To see how these attributes appear in the Gradle build script for PhpStorm, see [](dev_alternate_products.md#configuring-gradle-build-script-using-the-intellij-idea-product-attribute).

| `gradle-intellij-plugin` Attribute                                               | Attribute Value                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [`intellij.type`](tools_gradle_intellij_plugin.md#intellij-extension-type)       | `IU` for IntelliJ IDEA Ultimate. The required PHP plugin isn't compatible with IntelliJ IDEA Community Edition.                                                                                                      |
| [`intellij.version`](tools_gradle_intellij_plugin.md#intellij-extension-version) | Set to the same `IU` BRANCH.BUILD as the PhpStorm target version, e.g. `193.5233.102`.                                                                                                                               |
| [`intellij.plugins`](tools_gradle_intellij_plugin.md#intellij-extension-plugins) | `com.jetbrains.php:193.5233.102` for the PHP plugin.<br/>See below for PHP plugin version information.                                                                                                               |
| [`runIde.ideDir`](tools_gradle_intellij_plugin.md#tasks-runide-idedir)            | Path to locally installed target version of PhpStorm. For example, on macOS:<br/><path>/Users/$USERNAME$/Library/Application Support/JetBrains/Toolbox/apps/PhpStorm/ch-0/193.5233.101/PhpStorm.app/Contents</path>. |

The PHP plugin version is explicitly declared because it isn't bundled with IntelliJ IDEA Ultimate Edition.
Select a [version](https://plugins.jetbrains.com/plugin/6610-php/versions) of the PHP plugin compatible with the [`intellij.version`](tools_gradle_intellij_plugin.md#intellij-extension-version).

</tab>

</tabs>

The dependency on the PHP plugin APIs (`com.jetbrains.php`) must be declared in the <path>[plugin.xml](plugin_configuration_file.md)</path> file, as shown in the tutorial [Configuring plugin.xml](dev_alternate_products.md#configuring-pluginxml) section.
