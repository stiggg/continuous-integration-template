# Continuous Integration scripts for PHP project

This package includes ant build script for running

- code metrics
- documentation generation
- automated tests

in command line and Jenkins CI software. Symfony2 support is offered out of the box, for other frameworks you have to customize the build.xml.

## Command line usage

1. Install QA tools from PEAR:
 `pear config-set auto_discover 1`
 `pear install pear.phpqatools.org/phpqatools`
2. Install ant
3. Copy or symlink build.xml and build/ directory into project root folder.
4. Modify script to point to your sources folder (src/ by default)
5. Run specific build targets. Currently these command line targets are configured:
* `lint`: Syntax check source files
* `phploc`: Measure project size using PHPLOC
* `phpmd`: Perform project mess detection using PHPMD
* `phpcs`: Find coding standard violations using PHP_CodeSniffer (default standard is PSR-2)
* `phpcpd`: Find duplicate code using PHPCPD

So, `ant phpmd` executed at the root folder of your project runs php mess detector and outputs report to shell.

## Configuration

* PHPMD is configured by default to run with "Code Size", "Controversial" and "Design" rulesets. Add [more rulesets](https://github.com/phpmd/phpmd/tree/master/src/main/resources/rulesets) by editing build/phpmd.xml file.
* PHPCS is configured by default to check PSR-2 compliance. You can see installed rulesets by `phpcs -i`.

## Jenkins usage

Buildfile is modified from [http://jenkins-php.org](http://jenkins-php.org). Buildfile runs code metrics, generates documentation and runs automated PHPUnit tests.

1. Goto [http://jenkins-php.org](http://jenkins-php.org). Follow the instructions and install php-template to your Jenkins.
2. Configure your project. Add build target "Invoke Ant", set targets as:
```
metrics-parallel
symfony-init
phpunit
```

Symfony initialization target installs vendors and clears/warm-ups cache.

Additionally, there's `jasmine-node` target for running jasmine Javascript tests.
