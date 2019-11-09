# Islandora Bagger Integration

## Introduction

Drupal Module that allows user to create Bags with [Islandora Bagger](https://github.com/mjordan/islandora_bagger). Can be run in "remote" or "local" mode, as explained below.

## Requirements

* An [Islandora Bagger](https://github.com/mjordan/islandora_bagger) microservice
* [Islandora 8](https://github.com/Islandora-CLAW/islandora)
* [Context](https://www.drupal.org/project/context) if you want to define which Islandora Bagger configuration files to use other than the default file. A requirement of Islandora so will already be installed.

## Installation

1. Clone this Github repo into your Islandora's `drupal/web/modules/contrib` directory.
1. Enable the module either under the "Admin > Extend" menu or by running `drush en -y islandora_bagger_integration`.

## Configuration

The admin settings form for this module requires the following:

1. A choice of whether you are running it in local or remote mode.
1. The absolute path on your Drupal server to the default Islandora Bagger configuration file. This file is used if no Contexts are configured to use an alternative configuration file.
1. If using remote mode
   1. the URL of the Islandora Bagger REST endpoint. If you are running Islandora in the CLAW Vagrant, and Islandora Bagger on the host machine (i.e., same machine that is hosing the Vagrant), use `10.0.2.2` as your endpoint IP address instead of `localhost`.
   1. an option to add to the configuration file the email address of the user who requested the Bag be created. If checked, the user's email address will be added to the configuration file using the key `recipient_email`. In addition, if this option is checked, the message displayed to the user will indicate they will receive an email when their Bag is ready for download.
1. If running in local mode, the absolute path to the directory on your Drupal server where Islandora Bagger is installed.

After you configure the admin setting, place the "Islandora Bagger Block" as you normally would any other block. You should restrict this block to the content types of your Islandora nodes, and to user roles who you want to be able to create Bags.

## Usage

This module's interaction with Islanodra Bagger can be configured in two ways:

1. Using Islandora Bagger as a remote microservice ("remote" mode)
   * In this mode, submitting the "Create Bag" form does not directly generate the Bag; rather, it sends a request to the remote Islandora Bagger's REST interface, which in turn populates its processing queue with the node ID and the configuration file to use when that node's Bag is created.
   * In remote mode, this Drupal module does not notify the user when their Bag is ready; you will need to configure Islandora Bagger to send an email to the user indicating where they can get the Bag.
1. Using Islandora Bagger as command-line utility ("local" mode)
   * In this mode, submitting the "Create Bag" form calls out to Islandora Bagger on the server's shell, which then generates the Bag.

In both cases, end users generate a Bag for the current object by submitting a simple form (with just one button) in a block. If running in remote mode, the user is told that they will get an email indicating where they can download the Bag (the microservice needs to be configured to send this email); if running in local mode, the user is presented with a link where they can download the Bag.

The advantage of local mode is that the user is presented with the download link immediately after the Bag is generated. The disadvantage of the local mode is that creating the Bag is done synchronously, and there is a risk that, for objects that have very large files, the job will time out.

The advantage of the remote mode is that generating a Bag will never time out because clicking on the "Create Bag" button sends a simple REST request to the remote Islandora Bagger microservice, which then add the request to a queue to be processed later. This is also a disadvantage, since the user doesn't get to download the Bag until later.

## Using Context to define which configuration file to use

This module comes with a Context reaction that allows you to use Islandora Bagger configuration files other than the default. To enable this, do the folowing:

1. Install Context and Context UI modules (requirements for Islandora, so will already be done).
1. Create a Context or edit an existing Context.
1. Define your Conditions.
1. Add the "Islandora Bagger Config File" reaction.
1. Enter the absolute path on your Drupal server to the configuration file you want to use.

This module provides no mechanism for uploading configuration files via Drupal's web interface, so you will need access to the Drupal server's file system. Also, do not put configuration files in directories that are accessible via the web, since they contain credentials for accessing your Drupal's REST interface.

## To do

See issue list.

## Current maintainer

* [Mark Jordan](https://github.com/mjordan)

## License

[GPLv2](http://www.gnu.org/licenses/gpl-2.0.txt)
