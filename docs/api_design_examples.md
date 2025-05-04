# API Design Examples (API 设计示例)

This document provides simplified examples of the RESTful API endpoints that the Mobile Application might use to interact with the Backend Services via the API Gateway. These are illustrative and do not represent the complete API specification.

本文档提供了移动应用可能通过API网关与后端服务交互的RESTful API端点的简化示例。这些仅为说明性示例，不代表完整的API规范。

**Authentication:** All requests are assumed to require authentication (e.g., via a Bearer token in the `Authorization` header) handled by the API Gateway, which identifies the user making the request.
**认证：** 假定所有请求都需要通过API网关处理的认证（例如，在`Authorization`头中使用Bearer令牌），网关会识别发出请求的用户。

---

## 1. Get Personalized Agenda (获取个性化议程)

Retrieves the agenda tailored for the authenticated user, including their registered sessions and relevant general sessions.

获取为认证用户量身定制的议程，包括其已注册的会议和相关通用会议。

*   **Endpoint:** `GET /api/v1/agenda`
*   **Request Body:** None
*   **Successful Response (200 OK):**

```json
{
  "userId": "usr_123abc",
  "days": [
    {
      "date": "2024-11-15",
      "schedule": [
        {
          "sessionId": "s_001",
          "startTime": "09:00",
          "endTime": "10:00",
          "title": "Opening Ceremony & CEO Keynote",
          "location": "Main Hall A",
          "speaker": "Dr. Evelyn Reed",
          "isRegistered": true, // User is implicitly registered for keynotes
          "type": "keynote"
        },
        {
          "sessionId": "s_101",
          "startTime": "11:00",
          "endTime": "12:00",
          "title": "Parallel Track: Advanced AI Algorithms",
          "location": "Room 201",
          "speaker": "Dr. Alice Chen",
          "isRegistered": true, // User explicitly registered for this
          "type": "track_session"
        },
        {
          "sessionId": "s_102",
          "startTime": "11:00",
          "endTime": "12:00",
          "title": "Parallel Track: AI in Marketing",
          "location": "Room 202",
          "speaker": "Mr. Bob Williams",
          "isRegistered": false,
          "type": "track_session"
        }
        // ... more sessions
      ]
    },
    {
      "date": "2024-11-16",
      "schedule": [
        // ... schedule for day 2
      ]
    }
  ]
}
