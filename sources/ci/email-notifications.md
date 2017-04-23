main_section: CI
sub_section: Sending notifications
page_title: Sending email notifications
page_description: Configure Email to send notifications for Continuous Integration and Delivery actions
page_keywords: email, Continuous Integration, Continuous Deployment, CI/CD, testing, automation

#Sending CI notifications to Email

By default, we send email notifications to the last committer and project owner when a build fails, or the status changes from failed to passed.

We get your email address from your source control management system (GitHub/Bitbucket). To change the email address we send notifications to, you should [change your account email.](../getting-started/set-email/)

To customize email notifications, you'll need to configure options in the `shippable.yml` file.

##Basic config

Follow instructions below to configure email notifications for your CI workflows. This is configured in `shippable.yml`.

The yml format looks like this:

```
integrations:
  notifications:
    - integrationName: email
      type: email
      recipients:
        - exampleone@org.com
        - exampletwo@org.com
```

Use the descriptions of each field below to modify the `yml` above and tailor it to your requirements.

- `integrationName` value is always `email` since it is not configured in the UI - 'Account Settings' and 'Subscription' settings.
- `type` is `email`.
- `recipients` specifies the email addresses you want to send build status notifications to. This overrides the default setting of 'last committer' and 'project owner(s)' email address that we get from your source control management system (GitHub/Bitbucket). NOTE: We do not use the email address specified in your 'Account Settings' for notifications.
     - To specify 'last committer' and 'project owner(s)' as part of this list, you can use --last_committer and --owners.
     - For single recipient, use the format `recipients: example@org.com`

Check our blog ["Notifying CI failure/success status on Email and Slack"](http://blog.shippable.com/notifying-ci-failure/success-status-on-email-slack) for multiple scenarios.

##Advanced config

###1. Limiting branches

By default, Slack notifications are sent for builds for all branches. If you want to only send notifications for specific branch(es), you can do so with the `branches` keyword.

```
integrations:                               
  notifications:
    - integrationName: email
      type: email
      recipients:
        - exampleone@org.com
        - exampletwo@org.com
      branches:
        only:
          - master
```

`branches` allows you to choose the branches you want to send notifications for. The `only` tag should be used when you want to send notifications for builds of specific branches. You can also use the `except` tag to exclude specific branches. [Wildcards](../../ci/advancedOptions/branches/) are also supported.


###2. Customizing notification triggers

By default, Slack notifications are sent for the following events:

- <i class="ion-ios-minus-empty"></i> A build fails
- <i class="ion-ios-minus-empty"></i> Build status for a project changes from failed to passed
- <i class="ion-ios-minus-empty"></i> A Pull request is built

<br>
You can further customize these defaults with the following config:

```
integrations:                               
  notifications:
    - integrationName: email
      type: email
      recipients:
        - exampleone@org.com
        - exampletwo@org.com
      on_success: always | change | never
      on_failure: always | change | never
      on_start: always | never
      on_pull_request: always | never

```

You can set the following options for the `on_success`, `on_failure`, `on_start` and `on_pull_request` tags:

- <i class="ion-ios-minus-empty"></i>`always` means that you will always receive a notification for that event.

- <i class="ion-ios-minus-empty"></i> `never` means that you will never receive a notification for that event.

- <i class="ion-ios-minus-empty"></i> `change` for `on_success` or `on_failure` fields means you will receive notifications only when the build status changes to success or failure respectively. This value isn't supported for `on_start` or `on_pull_request`.

If you do not specify any of these tags, the defaults are: `on_success` is set to `change`, `on_failure` is set to `always`, `on_start` is set to `never` and `on_pull_request` is set to `always`.

###Turn off email notifications
If you do not want to get notified for any reason, you can turn off email notifications with the following in your `shippable.yml`:

```
integrations:                               
  notifications:
    - integrationName: email
      type: email
      on_success: never
      on_failure: never
      on_pull_request: never
```
---