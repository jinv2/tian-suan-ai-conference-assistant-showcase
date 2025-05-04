# AI Model Integration (AI 模型集成)

The TianSuan AI Conference Assistant leverages several types of AI models to deliver its intelligent features. This document conceptually describes the key AI components and their roles.

天算AI会议助手利用多种AI模型来实现其智能功能。本文档从概念上描述关键的AI组件及其作用。

## 1. Fine-tuned Large Language Model (LLM - 微调大语言模型)

*   **Core Technology:** Based on a powerful foundation LLM (e.g., TianSuan's proprietary model, or a fine-tuned version of models like Llama 3, Qwen, etc.).
*   **Fine-tuning:** The base LLM is specifically fine-tuned on a dataset curated from *this* conference's materials (agenda, speaker bios, report summaries, location details, pre-defined Q&A pairs). This makes the model an "expert" on the event.
*   **Key Functions:**
    *   **Intelligent Q&A:** Answering attendee questions about the schedule, speakers, locations, logistics, content ("What time is the CEO's keynote?", "Where is the Gala Dinner?", "Summarize the main points of the AI ethics report.").
    *   **Content Summarization:** Generating summaries of reports or session transcripts (on demand or automated).
    *   **Content Generation (Assisted):** Potentially assisting in drafting announcements or simple reports based on structured data.
*   **Retrieval-Augmented Generation (RAG):** To ensure answers are accurate and grounded in the latest conference data, the LLM service employs RAG. Before generating an answer, it retrieves relevant information snippets from the structured (SQL) and unstructured (Vector DB) knowledge bases based on the user's query. This retrieved context is then fed into the LLM's prompt, significantly reducing hallucinations and improving factual accuracy.

*   **核心技术：** 基于强大的基础LLM（如天算自研模型，或Llama 3、Qwen等模型的微调版本）。
*   **微调：** 基础LLM在根据**本次**会议资料（议程、嘉宾简介、报告摘要、地点详情、预定义问答对）整理的数据集上进行专门微调，使模型成为本次活动的“专家”。
*   **关键功能：**
    *   **智能问答：** 回答参会者关于日程、嘉宾、地点、后勤、内容的问题（“CEO的主题演讲几点开始？”，“晚宴在哪里？”，“总结一下AI伦理报告的要点？”）。
    *   **内容摘要：** 生成报告或会议录音的摘要（按需或自动）。
    *   **内容生成（辅助）：** 可能辅助基于结构化数据起草通知或简单报告。
*   **检索增强生成 (RAG):** 为确保回答准确并基于最新的会议数据，LLM服务采用RAG。在生成回答前，系统根据用户查询的嵌入向量从结构化（SQL）和非结构化（向量数据库）知识库中检索相关信息片段。这些检索到的上下文被注入到LLM的提示中，显著减少幻觉并提高事实准确性。

## 2. Computer Vision (CV - 计算机视觉)

*   **AI Check-in:**
    *   **Technology:** Facial recognition model.
    *   **Process:** Attendees opt-in and register their face via the app beforehand. Upon arrival, they briefly face a designated camera (or use the app's camera). The CV service matches the face against the registered database for seamless, contactless check-in. QR code scanning serves as a backup/alternative.
    *   **Considerations:** Strong emphasis on data privacy, consent, and security of biometric data. Data typically deleted post-event.
*   **Security Monitoring (Optional/Conceptual):**
    *   **Technology:** Object detection, anomaly detection, crowd analysis models running on venue camera feeds.
    *   **Functionality:** Can identify potential issues like overcrowding in specific areas, unauthorized access to restricted zones, or unusual activity patterns, triggering alerts for the minimal on-site security/operations team. This is primarily for situational awareness and rapid response coordination.

*   **AI签到：**
    *   **技术：** 人脸识别模型。
    *   **流程：** 参会者选择同意并在会前通过APP注册人脸信息。抵达时，短暂面对指定摄像头（或使用APP摄像头），CV服务将人脸与注册库比对，实现无缝、无接触签到。二维码扫描作为备用/替代方案。
    *   **考虑因素：** 强调数据隐私、用户同意和生物特征数据的安全性。数据通常在会后删除。
*   **安防监控（可选/概念性）：**
    *   **技术：** 基于会场摄像头视频流的目标检测、异常检测、人群分析模型。
    *   **功能：** 可识别潜在问题，如特定区域过度拥挤、未经授权进入限制区域、异常活动模式等，为极少数现场安保/运营人员触发警报。主要用于态势感知和快速响应协调。

## 3. Natural Language Processing (NLP - 自然语言处理)

*   **Speech-to-Text (STT):**
    *   **Functionality:** Transcribes audio recordings from conference sessions into text, enabling searchable archives and providing input for summarization.
*   **Text Summarization:**
    *   **Functionality:** (Can be part of the main LLM or a separate specialized model) Condenses session transcripts or long documents into key takeaways.
*   **Sentiment Analysis:**
    *   **Functionality:** Analyzes text feedback submitted by attendees (e.g., session ratings comments, overall feedback) to gauge satisfaction levels and identify common themes or concerns automatically.

*   **语音转文本 (STT):**
    *   **功能：** 将会议的录音转录为文本，实现可搜索的存档，并为摘要提供输入。
*   **文本摘要：**
    *   **功能：** （可作为主LLM的一部分或独立的专门模型）将会议记录或长文档浓缩为关键要点。
*   **情感分析：**
    *   **功能：** 分析参会者提交的文本反馈（如会议评分评论、总体反馈），自动评估满意度水平并识别普遍主题或关注点。

## 4. Recommendation Engine (推荐引擎)

*   **Functionality:** Provides personalized suggestions to enhance the attendee experience.
*   **Recommendations:**
    *   **Session Recommendations:** Suggests parallel track sessions based on the attendee's profile (job role, interests stated), past attendance, or explicit preferences.
    *   **Networking Recommendations:** Suggests other attendees to connect with based on shared interests, department, project involvement, or other relevant criteria (requires opt-in and privacy controls).
*   **Technology:** Can range from simple content-based filtering (matching keywords) to more complex collaborative filtering or graph-based methods if sufficient interaction data is available.

*   **功能：** 提供个性化建议以提升参会者体验。
*   **推荐内容：**
    *   **议程推荐：** 根据参会者的资料（职位、声明的兴趣）、过往参会记录或明确偏好，推荐平行分论坛。
    *   **社交推荐：** 基于共同兴趣、部门、项目参与或其他相关标准，推荐可联系的其他参会者（需要用户选择同意并设有隐私控制）。
*   **技术：** 可从简单的基于内容的过滤（匹配关键词）到更复杂的协同过滤或基于图的方法（如果交互数据充足）。

These AI models work in concert, orchestrated by the backend services, to create a seamless, intelligent, and efficient conference experience.
这些AI模型协同工作，由后端服务进行编排，共同创造无缝、智能、高效的会议体验。
