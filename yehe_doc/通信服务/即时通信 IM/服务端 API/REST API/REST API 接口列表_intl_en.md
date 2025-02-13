
## Account Management

| Feature | API |
|---------|---------|
| Imports a single account | [v4/im_open_login_svc/account_import](https://intl.cloud.tencent.com/document/product/1047/34953) |
| Imports multiple accounts | [v4/im_open_login_svc/multiaccount_import](https://intl.cloud.tencent.com/document/product/1047/34954) |
| Deletes accounts | [v4/im_open_login_svc/account_delete](https://intl.cloud.tencent.com/document/product/1047/34955) |
| Queries accounts | [v4/im_open_login_svc/account_check](https://intl.cloud.tencent.com/document/product/1047/34956) |
| Invalidates account login states  | [v4/im_open_login_svc/kick](https://intl.cloud.tencent.com/document/product/1047/34957) |
| Queries account online status | [ v4/openim/querystate](https://intl.cloud.tencent.com/document/product/1047/35477) |

## One-to-One Chat Message

| Feature | API |
|---------|---------|
| Sends one-to-one chat messages to one user | [v4/openim/sendmsg](https://intl.cloud.tencent.com/document/product/1047/34919) |
| Sends one-to-one chat messages to multiple users | [v4/openim/batchsendmsg](https://intl.cloud.tencent.com/document/product/1047/34920) |
| Imports one-to-one chat messages | [v4/openim/importmsg](https://intl.cloud.tencent.com/document/product/1047/35014) |
| Queries one-to-one chat messages | [v4/openim/admin_getroammsg](https://intl.cloud.tencent.com/document/product/1047/35478) |
| Recalls one-to-one chat messages | [v4/openim/admin_msgwithdraw](https://intl.cloud.tencent.com/document/product/1047/35015) |
| Marks one-to-one chat messages as read |[v4/openim/admin_set_msg_read](https://intl.cloud.tencent.com/document/product/1047/38996)|

## Pushing to All Users

| Feature | API |
|---------|---------|
| Pushes to all users         | [v4/all_member_push/im_push](https://intl.cloud.tencent.com/document/product/1047/37166) |
| Sets application attribute names | [v4/all_member_push/im_set_attr_name](https://intl.cloud.tencent.com/document/product/1047/37167) |
| Gets application attribute names | [v4/all_member_push/im_get_attr_name](https://intl.cloud.tencent.com/document/product/1047/37168) |
| Gets user attributes     | [v4/all_member_push/im_get_attr](https://intl.cloud.tencent.com/document/product/1047/37169) |
| Sets user attributes     | [v4/all_member_push/im_set_attr](https://intl.cloud.tencent.com/document/product/1047/37170) |
| Deletes user attributes     | [v4/all_member_push/im_remove_attr](https://intl.cloud.tencent.com/document/product/1047/37171) |
| Gets user tags     | [v4/all_member_push/im_get_tag](https://intl.cloud.tencent.com/document/product/1047/37172) |
| Adds user tags     | [v4/all_member_push/im_add_tag](https://intl.cloud.tencent.com/document/product/1047/37173) |
| Deletes user tags     | [v4/all_member_push/im_remove_tag](https://intl.cloud.tencent.com/document/product/1047/37174) |
| Deletes all tags of a user | [v4/all_member_push/im_remove_all_tags](https://intl.cloud.tencent.com/document/product/1047/37175) |

## Profile Management

| Feature | API |
| -------- | ------------------------------------------------------------ |
| Configures profiles | [v4/profile/portrait_set](https://intl.cloud.tencent.com/document/product/1047/34916) |
| Pulls profiles | [v4/profile/portrait_get](https://intl.cloud.tencent.com/document/product/1047/34917) |

## Relationship Chain Management

| Feature | API |
| -------- | ------------------------------------------------------------ |
| Adds friends | [v4/sns/friend_add](https://intl.cloud.tencent.com/document/product/1047/34902) |
| Imports friends | [v4/sns/friend_import](https://intl.cloud.tencent.com/document/product/1047/34903) |
| Updates friends | [v4/sns/friend_update](https://intl.cloud.tencent.com/document/product/1047/34904) |
| Deletes friends | [v4/sns/friend_delete](https://intl.cloud.tencent.com/document/product/1047/34905) |
| Deletes all friends | [v4/sns/friend_delete_all](https://intl.cloud.tencent.com/document/product/1047/34906) |
| Verifies friends | [v4/sns/friend_check](https://intl.cloud.tencent.com/document/product/1047/34907) |
| Pulls friends | [v4/sns/friend_get](https://intl.cloud.tencent.com/document/product/1047/34908) |
| Pulls specified friends | [v4/sns/friend_get_list](https://intl.cloud.tencent.com/document/product/1047/34910) |
| Blocklists users | [v4/sns/black_list_add](https://intl.cloud.tencent.com/document/product/1047/34911) |
| Unblocklists users | [v4/sns/black_list_delete](https://intl.cloud.tencent.com/document/product/1047/34912) |
| Pulls a blocklist | [v4/sns/black_list_get](https://intl.cloud.tencent.com/document/product/1047/34914) |
| Verifies users on a blocklist | [v4/sns/black_list_check](https://intl.cloud.tencent.com/document/product/1047/34913) |
| Adds lists | [v4/sns/group_add](https://intl.cloud.tencent.com/document/product/1047/34950) |
| Deletes lists | [v4/sns/group_delete](https://intl.cloud.tencent.com/document/product/1047/34926) |



## Group Management

| Feature | API |
| ---------------------- | ------------------------------------------------------------ |
| Creates a group | [v4/group_open_http_svc/create_group](https://intl.cloud.tencent.com/document/product/1047/34895) |
| Gets group profiles | [v4/group_open_http_svc/get_group_info](https://intl.cloud.tencent.com/document/product/1047/34961) |
| Gets group member profiles | [v4/group_open_http_svc/get_group_member_info](https://intl.cloud.tencent.com/document/product/1047/34948) |
| Modifies the profile of a group | [v4/group_open_http_svc/modify_group_base_info](https://intl.cloud.tencent.com/document/product/1047/34962) |
| Adds group members | [v4/group_open_http_svc/add_group_member](https://intl.cloud.tencent.com/document/product/1047/34921) |
| Deletes group members | [v4/group_open_http_svc/delete_group_member](https://intl.cloud.tencent.com/document/product/1047/34949) |
| Modifies the profile of a group member | [v4/group_open_http_svc/modify_group_member_info](https://intl.cloud.tencent.com/document/product/1047/34900) |
| Disbands a group | [v4/group_open_http_svc/destroy_group ](https://intl.cloud.tencent.com/document/product/1047/34896) |
| Gets the groups a user has joined | [v4/group_open_http_svc/get_joined_group_list](https://intl.cloud.tencent.com/document/product/1047/34925) |
| Queries the roles of users in a group | [v4/group_open_http_svc/get_role_in_group](https://intl.cloud.tencent.com/document/product/1047/34963) |
| Mutes/Unmutes group members | [v4/group_open_http_svc/forbid_send_msg](https://intl.cloud.tencent.com/document/product/1047/34951) |
| Gets the list of muted group members | [v4/group_open_http_svc/get_group_shutted_uin](https://intl.cloud.tencent.com/document/product/1047/34964) |
| Sends ordinary messages in a group | [v4/group_open_http_svc/send_group_msg](https://intl.cloud.tencent.com/document/product/1047/34959) |
| Sends system messages in a group | [v4/group_open_http_svc/send_group_system_notification](https://intl.cloud.tencent.com/document/product/1047/34958) |
| Recalls group messages | [v4/group_open_http_svc/group_msg_recall](https://intl.cloud.tencent.com/document/product/1047/34965) |
| Changes group owner | [v4/group_open_http_svc/change_group_owner](https://intl.cloud.tencent.com/document/product/1047/34966) |
| Imports a group profile | [v4/group_open_http_svc/import_group](https://intl.cloud.tencent.com/document/product/1047/34967) |
| Imports group messages | [v4/group_open_http_svc/import_group_msg ](https://intl.cloud.tencent.com/document/product/1047/34968) |
| Imports group members | [v4/group_open_http_svc/import_group_member](https://intl.cloud.tencent.com/document/product/1047/34969) |
| Sets the unread message count of a member | [v4/group_open_http_svc/set_unread_msg_num](https://intl.cloud.tencent.com/document/product/1047/34909) |
| Deletes messages sent by a specified user | [v4/group_open_http_svc/delete_group_msg_by_sender](https://intl.cloud.tencent.com/document/product/1047/34970) |
| Gets historical group messages | [v4/group_open_http_svc/group_msg_get_simple](https://intl.cloud.tencent.com/document/product/1047/34971) |
| Gets the number of online users in an audio-video group | [v4/group_open_http_svc/get_online_member_num](https://intl.cloud.tencent.com/document/product/1047/38521)|
| Queries the roles of users in a group | [v4/group_open_http_svc/get_role_in_group](https://intl.cloud.tencent.com/document/product/1047/34963)|

## Global Mute Management
| Feature | API |
|---------|---------|
| Sets global mute | [ v4/openconfigsvr/setnospeaking](https://intl.cloud.tencent.com/document/product/1047/34923) |
| Queries global mute | [ v4/openconfigsvr/getnospeaking](https://intl.cloud.tencent.com/document/product/1047/34924) |



## Operations Management

| Feature | API |
|---------|---------|
| Pulls operations data |[v4/openconfigsvr/getappinfo](https://intl.cloud.tencent.com/document/product/1047/34886) |
| Downloads recent messages  |[v4/open_msg_svc/get_history](https://intl.cloud.tencent.com/document/product/1047/34885) |
| Gets server IP addresses  |[v4/ConfigSvc/GetIPList](https://intl.cloud.tencent.com/document/product/1047/36742) |
