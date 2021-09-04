# docker-phpfpm PHP FastCGI Process Manager

A Docker container to provide a web service for executing applications written in PHP.

The container runs an instance of PHP FastCGI Process Manager for web applications written in PHP to display and retrieve data through standard web protocols. This container also enables PHP features for CMS applications (e.g. Drupal and Wordpress) like gd, mbstring, opache, pdo, mysql, and zip. Also this container adds extensions for development such as Xdebug and gdb.

This PHP-FPM container is intended to work as a part of an orchestrated bundle of web, database and mail service docker containers to provide a platform for developing and testing complete web applications on a laptop or desktop. The goal is to have an easy and quick way to start a web application using the docker-compose utility. For more information on how the platform is implemented see the [docker-phpweb](https://github.com/llslim/docker-phpweb) repository.

Security measures against malicious attacks is not a high priority for this container. Again, the intent of this container is for testing and development web applications on a personal laptop or desktop.  Using it in a production environment is NOT recommended.

## Mail services
This container is configured to use the [MSMTP Mail User Agent](http://msmtp.sourceforge.net/) to interpret sendmail calls from the web application and deliver messages to the mail service container.

The default sendmail_path in the php.ini is set to /usr/sbin/sendmail, which is a wrapper for the MSMTP user agent (via the msmtp_mta deb package).

Most often this container is paired with a [MailHog email testing tool for developers](https://github.com/mailhog/MailHog) container. By default the [MailHog container](https://hub.docker.com/r/mailhog/mailhog/) expose and listens to port 1025. The default configuration of MSMTP in the /etc/msmtprc file reflects that setting. The configuration can be change to work with more traditional Mail Transfer Agents like Postfix, Exim, or Mailgun by mounting a new msmtprc file through docker-compose.

## Error Logs
Error logging paths will be set to the /dev/stderr so docker can display them when we run the following on the command line:

`docker logs <container_name or ID>`.

## Base Container
The [Offical PHP Container](https://hub.docker.com/_/php/) is used as a base container.
