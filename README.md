# Overview

This charm (along with its companion, jenkins-slave) provides an easy way to deploy Jenkins on Ubuntu server and scale out Jenkins slaves.

This charm provides a Jenkins Server which can be accessed, after exposing, on http://<master>:8080.

# Usage

To deploy Jenkins server you will also need to deploy the jenkins-slave charm. This can be done as follows:

    juju deploy jenkins
    juju deploy -n 5 jenkins-slave
    juju add-relation jenkins jenkins-slave

The default password for the 'admin' account will be auto-generated.

You can set it using:

    juju set jenkins password=mypassword

Always change it this way - this account is used by the charm to manage slave configuration.

Then feel free to expose your Jenkins master:

    juju expose jenkins

The Jenkins UI will be accessible on http://<master>:8080

## Scale out Usage

The main method to use the Jenkins service at scale is to add units to the jenkins-slave, as illustrated in the example usage:

    juju deploy -n 5 jenkins-slave

Here the "-n 5" is adding 5 additional units (instances) to the jenkins-slave. Of course that "5" can be as large as you wish or you cloud provider supports. Additional information on scaling services with add-unit can be found at https://juju.ubuntu.com/docs/charms-scaling.html.


# Configuration

You have already seen the password configuration in the "Usage" section. Some other interesting config options are plugins and release. You can add config options via the command line with juju set or via a config file. More information on juju config is at: https://juju.ubuntu.com/docs/charms-config.html.

## Plugin config example

    juju set jenkins plugins=htmlpublisher view-job-filters bazaar git

## release config example

    juju set jenkins release=trunk

 
You could also set these config options via a config.yaml on jenkins deploy. For example your config.yaml could look like

    jenkins:
      plugins: htmlpublisher view-job-filters bazaar git 
      release: trunk 

You would then deploy jenkins with your config such as:

    juju deploy --config config.yaml jenkins
 
## Extending this charm

If you wish to perform custom configuration of either the master
or slave services, you can branch this charm and add install hooks
into hooks/install.d.

These will be executed when the main install, config-changed or
upgrade-charm hooks are executed (as the config-changed and
upgrade-charm hooks just call install)..

Additional hooks are executed in the context of the install hook
so may use any variables which are defined in this hook.

# Upstream Project Information 

- Upstream website: http://jenkins-ci.org/
- Upstream bug tracker: https://wiki.jenkins-ci.org/display/JENKINS/Issue+Tracking
- Upstream mailing list or contact information: http://jenkins-ci.org/content/mailing-lists
- Jenkins Plugins: https://wiki.jenkins-ci.org/display/JENKINS/Plugins
