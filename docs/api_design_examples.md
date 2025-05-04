# API 设计示例 (API Design Examples)

本文档提供了移动应用可能通过API网关与后端服务交互的RESTful API端点的简化示例。这些仅为说明性示例，不代表完整的API规范。

**认证：** 假定所有请求都需要通过API网关处理的认证（例如，在`Authorization`头中使用Bearer令牌），网关会识别发出请求的用户。

---

## 1. 获取个性化议程 (Get Personalized Agenda)

获取为认证用户量身定制的议程，包括其已注册的会议和相关通用会议。

*   **端点 (Endpoint):** `GET /api/v1/agenda`
*   **请求体 (Request Body):** 无
*   **成功响应 (200 OK):**

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
          "title": "开幕式及CEO主题演讲",
          "location": "主会场A厅",
          "speaker": "Dr. Evelyn Reed",
          "isRegistered": true, // 用户默认注册主旨演讲
          "type": "keynote"
        },
        {
          "sessionId": "s_101",
          "startTime": "11:00",
          "endTime": "12:00",
          "title": "平行分论坛：高级AI算法",
          "location": "会议室 201",
          "speaker": "Dr. Alice Chen",
          "isRegistered": true, // 用户明确注册此分论坛
          "type": "track_session"
        },
        {
          "sessionId": "s_102",
          "startTime": "11:00",
          "endTime": "12:00",
          "title": "平行分论坛：AI在市场营销中的应用",
          "location": "会议室 202",
          "speaker": "Mr. Bob Williams",
          "isRegistered": false,
          "type": "track_session"
        }
        // ... 更多会议安排
      ]
    },
    {
      "date": "2024-11-16",
      "schedule": [
        // ... 第二天的日程安排
      ]
    }
  ]
}

2. 向AI助手提问 (Ask the AI Assistant)

将用户的问题发送给AI助手（LLM后端）。

端点 (Endpoint): POST /api/v1/ai/ask

请求体 (Request Body):

{
  "query": "晚宴几点开始？",
  "chatHistory": [ // 可选：包含之前的对话轮次以提供上下文
    {"role": "user", "content": "主会场在哪里？"},
    {"role": "assistant", "content": "主会场位于一楼，主入口左侧。"}
  ]
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Json
IGNORE_WHEN_COPYING_END

成功响应 (200 OK):

{
  "answer": "晚宴于11月15日晚上19:00（晚上7点）在大宴会厅开始。",
  "confidenceScore": 0.95, // 可选：回答置信度指示
  "source": "议程元数据" // 可选：主要信息来源指示
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Json
IGNORE_WHEN_COPYING_END

错误响应 (例如, 400 Bad Request 如果查询为空):

{
  "error": "查询不能为空。"
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Json
IGNORE_WHEN_COPYING_END
3. 获取导航路线 (Get Navigation Route)

请求会场内两点之间的步行路线。

端点 (Endpoint): GET /api/v1/navigation?from=current_location&to=room_201

(查询参数 from 和 to 可以是预定义的地点ID、房间名称，或者可能是 'current_location'，后端根据用户最后已知位置/签到点解析)

请求体 (Request Body): 无

成功响应 (200 OK):

{
  "from": {
    "id": "loc_lobby",
    "name": "主大厅"
  },
  "to": {
    "id": "room_201",
    "name": "会议室 201"
  },
  "route": [ // 路径点或指令的有序列表
    {"type": "instruction", "text": "经过签到台直行。"},
    {"type": "waypoint", "coordinates": [120.1234, 30.5678], "floor": 1},
    {"type": "instruction", "text": "在咖啡站左转。"},
    {"type": "instruction", "text": "乘扶梯上二楼。"},
    {"type": "waypoint", "coordinates": [120.1240, 30.5680], "floor": 2},
    {"type": "instruction", "text": "会议室 201 在您的右侧。"},
    {"type": "destination", "coordinates": [120.1245, 30.5682], "floor": 2}
  ],
  "estimatedTime": "3 分钟"
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Json
IGNORE_WHEN_COPYING_END
4. 执行AI签到 (Perform AI Check-in)

使用人脸识别数据发起签到流程（概念性 - 实际实现可能不同）。

端点 (Endpoint): POST /api/v1/checkin

请求体 (Request Body): (可能包含图像数据或其引用)

{
  "imageData": "base64_encoded_image_data_or_image_url"
  // 如果在签到设备上进行，则可能包含设备上下文
  // "kioskId": "kiosk_entrance_A"
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Json
IGNORE_WHEN_COPYING_END

成功响应 (200 OK):

{
  "status": "success",
  "userId": "usr_123abc",
  "userName": "张三",
  "message": "签到成功。欢迎！",
  "timestamp": "2024-11-15T08:05:30Z"
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Json
IGNORE_WHEN_COPYING_END

失败响应 (例如, 401 Unauthorized 如果人脸未识别, 409 Conflict 如果已签到):

{
  "status": "failed",
  "error": "人脸未识别或未注册。",
  "timestamp": "2024-11-15T08:06:15Z"
}
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
Json
IGNORE_WHEN_COPYING_END
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
IGNORE_WHEN_COPYING_END
