+++
date = '2025-03-05T17:04:24+05:30'
draft = false
title = 'Instagram API with Instagram Login'
+++
## Information about the instagram api
started working for this at march 5 , 17: 05 to find out more about how it all works
<!--more-->
### Instagram API with Instagram Login
availbe For businesses to manage their presense on instagram

The API can be used to:


- Comment moderation – Manage and reply to comments on their media
- Content publishing – Get and publish their media
- Media Insights - Get insights on their media
- Mentions – Identify media where they have been `@mentioned` by other Instagram users
- Messaging – Send and receive messages with customers or people interested in their Instagram account

The above information sourced from: [Instagram API](https://developers.facebook.com/docs/instagram-platform/instagram-api-with-instagram-login)


The Instagram Platform is a collection of APIs that allows your app to access data for Instagram professional accounts including both businesses and creators. You can build an app that only serves `your Instagram professional account`, or you can build an app that `serves other Instagram professional accounts` that you do not own or manage.

# Instagram Platform API Integration Guide

Welcome to the Instagram Platform API Integration Guide! This document provides a comprehensive overview of how to integrate Instagram's APIs into your application, enabling access to data for Instagram professional accounts, including businesses and creators. Whether you're building an app for your own Instagram professional account or for others, this guide will help you understand the necessary configurations, access levels, and features.

## Table of Contents
1. [Introduction](#introduction)
2. [API Configurations](#api-configurations)
3. [Access Levels](#access-levels)
4. [App Review](#app-review)
5. [App Users](#app-users)
6. [Authentication and Authorization](#authentication-and-authorization)
7. [Features and Permissions](#features-and-permissions)

## Introduction
The Instagram Platform offers a suite of APIs that allow your application to interact with Instagram professional accounts. These APIs enable functionalities such as content publishing, comment moderation, messaging, and insights. Depending on your app's requirements, you can choose between two API configurations: one that uses Facebook Login for Business and another that uses Business Login for Instagram.

## API Configurations
### Instagram API with Facebook Login for Business
- **Use Case**: Your app serves Instagram professional accounts linked to a Facebook Page.
- **Login Method**: Users log in with their Facebook credentials.
- **Features**: Comment moderation, content publishing, insights, messaging via Messenger Platform, and more.

### Instagram API with Business Login for Instagram
- **Use Case**: Your app serves Instagram professional accounts with a presence on Instagram only.
- **Login Method**: Users log in with their Instagram credentials.
- **Features**: Comment moderation, content publishing, insights, messaging, and more.

## Access Levels
### Standard Access
- **Default Access Level**: Available to all apps.
- **Intended Use**: For apps used by people with roles on them, during development, or for testing.
- **Limitations**: Limited data access; suitable for apps serving your own or managed Instagram professional accounts.

### Advanced Access
- **Required For**: Apps serving Instagram professional accounts not owned or managed by you.
- **Requirements**: App Review and Business Verification.
- **Benefits**: Full access to API features and data.

## App Review
Meta App Review is a process that verifies your app's compliance with Meta's policies. Completing this review is necessary to gain Advanced Access. If your app is private or lacks a user interface, you can request approval for specific permissions like `instagram_basic` and `instagram_manage_comments`.

## App Users
To use the Instagram APIs, your app users must have an Instagram professional account, which can be for a business or creator. Depending on the API configuration, users may need to log in with either Instagram or Facebook credentials. For accounts linked to a Facebook Page, users must also have admin-equivalent tasks on the linked Page.

## Authentication and Authorization
### Login Flow
1. **User Clicks Embed URL**: Meta opens an authorization window.
2. **User Grants Permissions**: Meta redirects the user to your app’s redirect URI with an Authorization Code.
3. **Exchange Authorization Code**: Obtain a short-lived access token, user ID, and granted permissions.
4. **Obtain Long-Lived Access Token**: Exchange the short-lived token for a long-lived token valid for 60 days.

### Access Tokens
- **Short-Lived Token**: Valid for one hour.
- **Long-Lived Token**: Valid for 60 days, refreshable.
- **Token Types**: Instagram User access tokens (for Instagram Login) or Facebook User access tokens (for Facebook Login).

## Features and Permissions
### Instagram Login
- `instagram_business_basic`
- `instagram_business_content_publish`
- `instagram_business_manage_comments`
- `instagram_business_manage_messages`
- `Human Agent`

### Facebook Login
- `instagram_basic`
- `instagram_content_publish`
- `instagram_manage_comments`
- `instagram_manage_insights`
- `instagram_manage_messages`
- `pages_show_list`
- `pages_read_engagement`
- `Human Agent`
- `Instagram Public Content Access`

The **Human Agent** feature allows your app to have a human agent respond to user messages within 7 days using the `human_agent` tag.
reference from: [Instagram API](https://developers.facebook.com/docs/instagram-platform/overview)

# Instagram Messaging API Guide

This guide provides detailed instructions on how to use the Instagram Messaging API to send and manage messages, reactions, and media shares. It assumes you have already set up the necessary components, such as a Meta login flow and a webhooks server to receive notifications.

## Table of Contents
1. [Requirements](#requirements)
2. [Access Tokens](#access-tokens)
3. [Base URL](#base-url)
4. [Endpoints](#endpoints)
5. [Required Parameters](#required-parameters)
6. [IDs](#ids)
7. [Permissions](#permissions)
8. [Webhook Event Subscriptions](#webhook-event-subscriptions)
9. [Media Types and Specifications](#media-types-and-specifications)
10. [Send a Text Message](#send-a-text-message)
11. [Send an Image or GIF](#send-an-image-or-gif)
12. [Send Audio or Video](#send-audio-or-video)
13. [Send a Sticker](#send-a-sticker)
14. [React or Unreact to a Message](#react-or-unreact-to-a-message)
15. [Send a Published Post](#send-a-published-post)

## Requirements
- **Access Level**:
  - **Advanced Access**: Required if your app serves Instagram professional accounts you don't own or manage.
  - **Standard Access**: Suitable if your app serves Instagram professional accounts you own or manage. Note that some features may not work properly until your app has been granted Advanced Access.
- **Access Tokens**: An Instagram User access token requested from a person who can send a message from the Instagram professional account.

## Access Tokens
- **Instagram User Access Token**: Required for API requests.

## Base URL
All endpoints can be accessed via the `graph.instagram.com` host.

## Endpoints
- `/<IG_ID>/messages` or `/me/messages`

## Required Parameters
- `recipient:{id:<IGSID>}`
- `message:{<MESSAGE_ELEMENTS>}`

## IDs
- **Instagram Professional Account ID (`<IG_ID>`)**: The ID of the app user's Instagram professional account that received the message.
- **Instagram-scoped ID (`<IGSID>`)**: The ID for the Instagram user who sent the message to your app user, received from an Instagram messaging webhook notification.

## Permissions
- `instagram_business_basic`
- `instagram_business_manage_messages`

## Webhook Event Subscriptions
- `messages`
- `messaging_optins`
- `messaging_postbacks`
- `messaging_reactions`
- `messaging_referrals`
- `messaging_seen`

## Media Types and Specifications
| Media Type | Supported Formats | Maximum Size |
|------------|-------------------|--------------|
| Audio      | aac, m4a, wav, mp4 | 25MB         |
| Image      | png, jpeg, gif    | 8MB          |
| Video      | mp4, ogg, avi, mov, webm | 25MB |

## Send a Text Message
To send a text message or a link, send a POST request to the `/<IG_ID>/messages` endpoint.

**Sample Request**:
```bash
curl -X POST "https://graph.instagram.com/v22.0/<IG_ID>/messages" \
     -H "Authorization: Bearer <INSTAGRAM_USER_ACCESS_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "recipient":{
               "id":"<IGSID>"
           },
           "message":{
              "text":"<TEXT_OR_LINK>"
           }
        }'
```

## Send an Image or GIF
To send an image or GIF, include an attachment object in the message parameter.

**Sample Request**:
```bash
curl -X POST "https://graph.instagram.com/v22.0/<IG_ID>/messages" \
     -H "Authorization: Bearer <INSTAGRAM_USER_ACCESS_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "recipient":{
               "id":"<IGSID>"
           },
           "message":{
              "attachment":{
               "type":"image",
               "payload":{
                 "url":"<IMAGE_OR_GIF_URL>"
               }
             }
           }
         }'
```

## Send Audio or Video
To send an audio or video message, set the attachment type to `audio` or `video`.

**Sample Request**:
```bash
curl -X POST "https://graph.instagram.com/v22.0/<IG_ID>/messages" \
     -H "Authorization: Bearer <INSTAGRAM_USER_ACCESS_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "recipient":{
               "id":"<IGSID>"
           },
           "message":{
              "attachment":{
               "type":"audio",  // Or set to video
               "payload":{
                 "url":"<AUDIO_OR_VIDEO_FILE_URL>"
               }
             }
          }
        }'
```

## Send a Sticker
To send a heart sticker, set the attachment type to `like_heart`.

**Sample Request**:
```bash
curl -X POST "https://graph.instagram.com/v22.0/<IG_ID>/messages" \
     -H "Authorization: Bearer <INSTAGRAM_USER_ACCESS_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "recipient":{
               "id":"<IGSID>"
           },
           "message":{
              "attachment":{
                "type":"like_heart"
              }
            }
         }'
```

## React or Unreact to a Message
To react or unreact to a message, use the `sender_action` parameter.

**Sample Request**:
```bash
curl -X POST "https://graph.instagram.com/v22.0/<IG_ID>/messages" \
     -H "Authorization: Bearer <INSTAGRAM_USER_ACCESS_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "recipient":{
               "id":"<IGSID>"
           },
           "sender_action":"react",  // Or set to unreact to remove the reaction
           "payload":{
             "message_id":"<MESSAGE_ID>",
             "reaction":"love"      // Omit if removing a reaction
           }
         }'
```

## Send a Published Post
To send a message containing an app user's Instagram post, set the attachment type to `MEDIA_SHARE`.

**Sample Request**:
```bash
curl -X POST "https://graph.instagram.com/v22.0/<IG_ID>/messages" \
     -H "Authorization: Bearer <INSTAGRAM_USER_ACCESS_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "recipient":{
               "id":"<IGSID>"
           },
           "message":{
              "attachment":{
                "type":"MEDIA_SHARE",
                "payload":{
                  "id":"<POST_ID>"
                }
              }
           }
        }'
```

This guide should help you effectively use the Instagram Messaging API to interact with Instagram professional accounts.


information sourced from [instagram messaging API](https://developers.facebook.com/docs/instagram-platform/instagram-api-with-instagram-login/messaging-api#requirements)

# Instagram Conversations API Guide

This guide explains how to retrieve information about conversations between your app user and an Instagram user interested in your app user's Instagram media. You can use this API to:

- Get a list of conversations for your app user's Instagram professional account.
- Retrieve a list of messages within each conversation.
- Access details about each message, including when it was sent and by whom.

---

## Table of Contents
1. [Requirements](#requirements)
2. [Access Tokens](#access-tokens)
3. [Base URL](#base-url)
4. [Endpoints](#endpoints)
5. [IDs](#ids)
6. [Permissions](#permissions)
7. [Limitations](#limitations)
8. [Get a List of Conversations](#get-a-list-of-conversations)
9. [Find a Conversation with a Specific Person](#find-a-conversation-with-a-specific-person)
10. [Get a List of Messages in a Conversation](#get-a-list-of-messages-in-a-conversation)
11. [Get Information About a Message](#get-information-about-a-message)

---

## Requirements
This guide assumes you have:
- Read the [Instagram Platform Overview](https://developers.facebook.com/docs/instagram-api/).
- Implemented the necessary components, such as a Meta login flow and a webhooks server to receive notifications.

### Access Level
- **Advanced Access**: Required if your app serves Instagram professional accounts you don't own or manage.
- **Standard Access**: Suitable if your app serves Instagram professional accounts you own or manage and have added to your app in the App Dashboard.

### Access Tokens
- **Instagram User Access Token**: Requested from a person who can manage messages on the Instagram professional account.

---

## Base URL
All endpoints can be accessed via the `graph.instagram.com` host.

---

## Endpoints
- `/<IG_ID>/conversations` or `/me/conversations`

---

## IDs
- **Instagram Professional Account ID (`<IG_ID>`)**:
  - The ID of the app user's Instagram professional account.
- **Instagram-scoped ID (`<IGSID>`)**:
  - The ID for the Instagram user in the conversation.

---

## Permissions
- `instagram_business_basic`
- `instagram_business_manage_messages`

---

## Limitations
- Only the image or video URL for a share will be included in the data returned in API calls or webhooks notifications.
- Conversations in the **Requests folder** that have not been active for **30 days** will not be returned in API calls.

---

## Get a List of Conversations
To retrieve a list of conversations for your app user's Instagram professional account, send a **GET** request to the `/<IG_ID>/conversations` endpoint.

### Sample Request
```bash
curl -i -X GET "https://graph.instagram.com/v22.0/me/conversations?platform=instagram&access_token=<INSTAGRAM_ACCESS_TOKEN>"
```

### Example Response
```json
{
  "data": [
    {
      "id": "<CONVERSATION_ID_1>",
      "updated_time": "<UNIX_TIMESTAMP>"
    },
    {
      "id": "<CONVERSATION_ID_2>",
      "updated_time": "<UNIX_TIMESTAMP>"
    }
  ]
}
```

---

## Find a Conversation with a Specific Person
To retrieve a conversation between your app user's Instagram professional account and a specific Instagram user, send a **GET** request to the `/<IG_ID>/conversations` endpoint with the `user_id` parameter set to the Instagram-scoped ID (`<IGSID>`).

### Sample Request
```bash
curl -i -X GET "https://graph.instagram.com/v22.0/me/conversations?user_id=<IGSID>&access_token=<INSTAGRAM_ACCESS_TOKEN>"
```

### Example Response
```json
{
  "data": [
    {
      "id": "<CONVERSATION_ID>"
    }
  ]
}
```

---

## Get a List of Messages in a Conversation
To retrieve a list of messages in a conversation, send a **GET** request to the `/<CONVERSATION_ID>` endpoint with the `fields` parameter set to `messages`.

### Sample Request
```bash
curl -i -X GET "https://graph.instagram.com/v22.0/<CONVERSATION_ID>?fields=messages&access_token=<INSTAGRAM_ACCESS_TOKEN>"
```

### Example Response
```json
{
  "messages": {
    "data": [
      {
        "id": "<MESSAGE_1_ID>",
        "created_time": "<UNIX_TIMESTAMP>"
      },
      {
        "id": "<MESSAGE_2_ID>",
        "created_time": "<UNIX_TIMESTAMP>"
      },
      {
        "id": "<MESSAGE_3_ID>",
        "created_time": "<UNIX_TIMESTAMP>"
      }
    ]
  },
  "id": "<CONVERSATION_ID>"
}
```

---

## Get Information About a Message
To retrieve details about a specific message, such as the sender, receiver, and message content, send a **GET** request to the `/<MESSAGE_ID>` endpoint with the `fields` parameter set to a comma-separated list of fields you are interested in.

### Default Fields
- `id`
- `created_time`

### Sample Request
```bash
curl -i -X GET "https://graph.instagram.com/v22.0/<MESSAGE_ID>?fields=id,created_time,from,to,message&access_token=<INSTAGRAM_ACCESS_TOKEN>"
```

### Example Response
```json
{
  "id": "<MESSAGE_ID>",
  "created_time": "2022-07-12T19:11:07+0000",
  "to": {
    "data": [
      {
        "username": "<IG_ID_USERNAME>",
        "id": "<IG_ID>"
      }
    ]
  },
  "from": {
    "username": "<IGSID_USERNAME>",
    "id": "<IGSID>"
  },
  "message": "Hi Kitty!"
}
```

---

## Notes
- Queries to the `/<CONVERSATION_ID>` endpoint will return all message IDs in a conversation. However, you can only retrieve details about the **20 most recent messages**. If you query a message older than the last 20, you will receive an error indicating that the message has been deleted.

---

information sourced from [instagram conversation API](https://developers.facebook.com/docs/instagram-platform/instagram-api-with-instagram-login/conversations-api)

---

### Handwritten Notes during the research:


Here are the handwritten notes about the Instagram Messaging API and Conversation API, presented in a clean and organized grid:

| ![Note 1 (https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_1.jpg)](https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_1.jpg) | ![Note 2 (https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_2.jpg)](https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_2.jpg) | ![Note 3 (https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_3.jpg)](https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_3.jpg) |
|:---:|:---:|:---:|
| Note 1 | Note 2 | Note 3 |

| ![Note 4 (https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_4.jpg)](https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_4.jpg) | ![Note 5 (https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_5.jpg)](https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_5.jpg) | ![Note 6 (https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_6.jpg)](https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_6.jpg) |
|:---:|:---:|:---:|
| Note 4 | Note 5 | Note 6 |

| ![Note 7 (https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_7.jpg)](https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_7.jpg) | ![Note 8 (https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_8.jpg)](https://cdn.jsdelivr.net/gh/shivamatwork16/img-processing@main/notes/instagram_api/Notes_instagram_API_8.jpg) | |
|:---:|:---:|:---:|
| Note 7 | Note 8 | |
