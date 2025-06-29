---
title: Zabbix send notification
description: How to configure notification send.
published: true
date: 2025-06-11T13:36:50.418Z
tags: linux
editor: markdown
dateCreated: 2025-06-11T13:12:14.264Z
---

# Zabbix 6.0 send notification

When you have some services and hosts you're monitoring, it's better when you get notificated if something goes to up or down state.

## Media types

> You have to go to `Administration/Media types` tab. Here you will find all preconfigured media types, and if you have created some then those too.
{.is-info}

### Email
> Look for one named **Email**, there are two from them, you can set whichever you want, the plain email will send a plain text, but the **Email (HTML)** will send a HTML formatted text through email. If you have choosen one (or you can use both), set these settings:
{.is-info}

- **SMTP Server**: Your SMTP server (or google or whatever you want to user)
- **SMTP server port**: Your SMTP servers port
- **SMTP helo**: Your domain tag
- **SMTP email**: The email that will be the sender email
- **Connection security**: Choose which you want and admint your SSL settings as needed
- **Authentication**: Username and password authentication and then fill the **Username** and **Password** field!

> CLICK UPDATE!!
{.is-warning}

> If you want to edit the body of the text that the email will send then change to tab `Message templates` and assing values and email body as you want!
{.is-info}


### Custom script

> If you don't see the option that would fit your logging don't worry! There is a way to do anything you want. On the `Media types` page choose **Create media type** in the top right corner. Give it a unique **name**, and choose **Type: Script**. In the field  **Script name** you will have to write the name of the script that you will create later into `/etc/zabbix/alert.d` directory. So if it will be *a.sh* then use here *a.sh* too!
You can add parameters to your script what will pass them to it. And write a description so if you see this one week later, know what you have done here with it!
{.is-info}

> Don't forget to add **Message template** otherwise the message body will be empty!
{.is-warning}


## User medias

> After you created or found and configured your media type, than you can add these to an user, with locations, where to send this information.
{.is-info}

- Go to `Administration\Users` tab.
- Choose or create a user for notifications.
- On **Media** tab at user configuration page, add the media types, that you will use. Set **Send to**, where you want to send it, and when to send it and which severity to send.
- After you have done that **CLICK the Update** button!

## Action trigger

>You can manage when to send email under `Configuration\Actions\Trigger actions`. Create a new action or edit and enable the default.
{.is-info}

### Action page

- **Name**: You can set up the name
- **Conditions**: You can set up conditions when to do the opeartions. If you want you can set up more, and after you have at least two conditions, you can set the **Type of calculation** how to handle these conditions.
- **Enabled**: You can enable or disable it.


### Operations page

- **Default operation step duration**: default 1h
- **Operations**: There are 3 operations, you can set who, or which group to send the notificaiton, and with which media type to send the notification.