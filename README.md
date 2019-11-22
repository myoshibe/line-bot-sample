# What is it?
A simple example of Looker Action Hub to send a message to your LINE bot.
([LINE](https://line.me/en-US/download) is one of the major messaging platforms in Japan.)

# Looker Action Hub References
- [What is Looker Action Hub?](https://docs.looker.com/ja/sharing-and-publishing/action-hub)
- [List of existing Looker Hosted Action Hub](https://docs.looker.com/ja/admin-options/platform/actions#list_of_integrated_services)
- [Looker Action Hub Github Repo](https://github.com/looker/actions/pulls)
- [Reference to Action Hub API](https://github.com/looker/actions/blob/master/docs/action_api.md)
- [Export the Results of a Looker Query to BigQuery](https://discourse.looker.com/t/export-the-results-of-a-looker-query-to-bigquery/9720) 

# How to deploy
1. [Line Business](https://developers.line.biz/ja/) to get a LINE Business account
2. [Heroku](https://www.heroku.com/home) to deploy Action Hub Server
3. Setting on your Looker instance
    - Register your Action Hub in the Admin panel
    - Embed action in the LookML

### 1. Line ID
1. [Register your new LINE Business Account](https://www.linebiz.com/jp/manual/OfficialAccountManager/app/tutorial/step1/)
2. [Log in](https://developers.line.biz/ja/) to the Developer console with your LINE Business Account
3. Set up a **Messenger API Channel** along with [this procedure](https://developers.line.biz/ja/docs/messaging-api/getting-started/)
4. You can manage your channel at [Console](https://developers.line.biz/console/).

`Channel access token (long-lived) `
<img width="1141" alt="Screen Shot 2019-11-16 at 9.07.30.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/d3025f1e-78a7-b974-e458-3cfabb5a40bd.png">
`Your user ID `
<img width="1132" alt="Screen Shot 2019-11-16 at 9.07.41.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/344b78ff-e70d-4fc7-3a1f-479c51733241.png">

5. Read a bot QR code `QR code of your bot` and add to your LINE friend

### 2. Deploy your Action Hub Server on Heroku
1. Access to [Heroku](https://www.heroku.com/home), if you don't have heroku account, create one from [<img width="72" alt="Screen Shot 2019-11-15 at 21.29.32.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/5be19d27-067c-f8a8-5d70-2c5271d9401f.png">](https://id.heroku.com/signup/login) 
2. Deploy this sample application to your heroku by clicking [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy) to deploy your Heroku. Names your application
3. Set your application vars at ```Settings``` > ```Reveal Config Vars```
<img width="612" alt="Screen Shot 2019-11-15 at 21.56.41.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/34a7b0e7-01e3-4976-6f38-b8bb8d0a2515.png">
4. Enter your ```Channel access token (long-lived) ``` and ```Your user ID ``` as an environment variable. Var names should be ```CHANNEL_ACCESS_TOKEN```, ```LINE_USER_ID```
<img width="1227" alt="Screen Shot 2019-11-15 at 22.02.57.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/97e8d2bd-c555-d00b-b2fa-4e5baff12b2f.png">

### 3. Looker Set Up
5. Access to https://{your_looker_instance}/admin/actions and set up your Action Hub Server, click <img width="115" alt="Screen Shot 2019-11-15 at 22.06.08.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/1c45fb53-687a-3c95-4218-6f0a157ae282.png"> button
6. Enter as ```https://{your_heroku_app_name}.herokuapp.com/action_list``` and click ```Add Action Hub```
<img width="1210" alt="Screen Shot 2019-11-15 at 22.07.35.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/bc07bcf8-a48b-1caf-d839-18987691f0e9.png">
7. Click `Enable` and `Save`
<img width="1188" alt="Screen Shot 2019-11-15 at 22.10.41.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/c668a75d-505a-88b7-7d20-2fbef67b87e6.png">
<img width="340" alt="Screen Shot 2019-11-15 at 22.11.45.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/124e765e-2726-7416-7531-ba39d87568e6.png">
8. Edit a dimension like: 

  ----

    dimension: name {
      type: string
      sql: ${TABLE}.name ;;
      action: {
        label: "Send to LINE"
        url: "https://{your_app_name}.herokuapp.com/action_execute"
        icon_url: "data:image/jpeg;base64,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"
        form_param: {
          name: "message"
          type: textarea
          required: yes
          default: "Thank you!!"
        }
      }
    }

9. Open an Explore and run query, you can find an action in the cell. Click `Send to LINE`

<img width="300" alt="Screen Shot 2019-11-16 at 9.23.22.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/a118fe43-1638-191b-b0ef-ccba140f5c5e.png">

10. Input some messages and click `Submit`
<img width="603" alt="Screen Shot 2019-11-16 at 9.24.47.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/61e618a7-4d32-7ea8-2ecc-efd8769f1224.png">
11. If you got a message, success!

![IMG_1898.PNG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/7bac16e3-52c9-96a6-7fa5-2a1db0fea549.png)

Above is an example of `cell` level action. You can create an action as a `query` level as well. If `query` level action is created, you can send LINE from the top of the gear icon in the Explore or Look.
<img width="952" alt="Screen Shot 2019-11-16 at 9.27.08.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/486566/d302ea6f-c775-8670-b767-f7f99cce313d.png">
