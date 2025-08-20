<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AI角色扮演微信模拟器</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: "PingFang SC", "Helvetica Neue", Arial, sans-serif;
      -webkit-tap-highlight-color: transparent;
    }
    
    body {
      background: linear-gradient(135deg, #ffd1dc, #ffc2d1);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
      color: #333;
    }
    
    /* 手机容器样式 */
    .phone-container {
      width: 390px;
      height: 844px;
      background: #f8f8f8;
      border-radius: 40px;
      overflow: hidden;
      box-shadow: 0 20px 50px rgba(0, 0, 0, 0.2);
      position: relative;
      display: flex;
      flex-direction: column;
      border: 12px solid #000;
    }
    
    /* 状态栏 */
    .status-bar {
      height: 44px;
      background: #f8f8f8;
      padding: 0 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      color: #000;
      font-size: 14px;
      border-bottom: 1px solid rgba(0,0,0,0.05);
    }
    
    /* 导航栏 */
    .nav-bar {
      height: 44px;
      background: #f8f8f8;
      padding: 0 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 1px solid rgba(0,0,0,0.1);
      font-size: 17px;
      font-weight: 500;
    }
    
    .nav-bar i {
      font-size: 20px;
      color: #07c160;
    }
    
    /* 主内容区域 */
    .main-content {
      flex: 1;
      overflow: hidden;
      position: relative;
    }
    
    /* 标签页切换 */
    .tab-content {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: none;
      background: #f0f0f0;
      overflow-y: auto;
    }
    
    .tab-content.active {
      display: block;
    }
    
    /* 底部标签栏 */
    .tab-bar {
      height: 60px;
      background: #f8f8f8;
      display: flex;
      border-top: 1px solid rgba(0,0,0,0.1);
    }
    
    .tab-item {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: #999;
      font-size: 12px;
      cursor: pointer;
    }
    
    .tab-item i {
      font-size: 24px;
      margin-bottom: 4px;
    }
    
    .tab-item.active {
      color: #07c160;
    }
    
    /* 聊天列表 */
    .chat-list {
      padding: 10px 0;
    }
    
    .chat-item {
      display: flex;
      padding: 12px 16px;
      background: #fff;
      position: relative;
      border-bottom: 1px solid #eee;
      cursor: pointer;
    }
    
    .chat-item:active {
      background: #f0f0f0;
    }
    
    .avatar {
      width: 48px;
      height: 48px;
      border-radius: 8px;
      margin-right: 12px;
      background: #07c160;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      font-size: 20px;
      overflow: hidden;
    }
    
    .avatar img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    
    .chat-info {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }
    
    .chat-name {
      font-size: 16px;
      font-weight: 500;
      margin-bottom: 6px;
    }
    
    .last-msg {
      font-size: 13px;
      color: #999;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    
    .chat-time {
      position: absolute;
      top: 16px;
      right: 16px;
      font-size: 12px;
      color: #999;
    }
    
    /* 自定义角色区域 */
    .setup-section {
      padding: 16px;
      background: #fff;
      margin-top: 10px;
    }
    
    .section-title {
      font-size: 16px;
      font-weight: 500;
      margin-bottom: 16px;
    }
    
    /* 上传控件 */
    .upload-area {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-bottom: 20px;
    }
    
    .avatar-upload {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      border: 2px dashed #ccc;
      display: flex;
      justify-content: center;
      align-items: center;
      margin-bottom: 10px;
      cursor: pointer;
      position: relative;
      overflow: hidden;
      background: #f9f9f9;
    }
    
    .avatar-upload i {
      font-size: 24px;
      color: #999;
    }
    
    .avatar-upload img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: none;
    }
    
    .avatar-upload.active img {
      display: block;
    }
    
    .avatar-upload.active i {
      display: none;
    }
    
    .upload-text {
      font-size: 14px;
      color: #999;
      text-align: center;
    }
    
    /* 表单元素 */
    .form-group {
      margin-bottom: 16px;
    }
    
    .form-label {
      display: block;
      margin-bottom: 8px;
      font-size: 14px;
      color: #666;
    }
    
    .form-input {
      width: 100%;
      padding: 12px;
      border: 1px solid #ddd;
      border-radius: 8px;
      font-size: 15px;
      outline: none;
    }
    
    .form-input:focus {
      border-color: #07c160;
    }
    
    .form-textarea {
      width: 100%;
      height: 100px;
      padding: 12px;
      border: 1px solid #ddd;
      border-radius: 8px;
      resize: none;
      font-size: 15px;
      outline: none;
    }
    
    /* 按钮样式 */
    .btn {
      padding: 12px 20px;
      border-radius: 8px;
      font-size: 16px;
      font-weight: 500;
      border: none;
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
      transition: all 0.2s;
    }
    
    .btn:active {
      transform: scale(0.98);
    }
    
    .btn-primary {
      background: #07c160;
      color: white;
      width: 100%;
    }
    
    .btn-test {
      background: #3a86ff;
      color: white;
      width: 100%;
      margin-bottom: 10px;
    }
    
    /* 设置按钮 */
    .settings-btn {
      position: absolute;
      right: 16px;
      top: 12px;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      background: #f0f0f0;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      z-index: 10;
    }
    
    /* 朋友圈区域 */
    .moment-cover {
      height: 200px;
      background: linear-gradient(rgba(0,0,0,0.3), rgba(0,0,0,0.3)), url('https://images.unsplash.com/photo-1493246507139-91e8fad9978e?ixlib=rb-4.0.3') center/cover;
      position: relative;
    }
    
    .cover-upload {
      position: absolute;
      bottom: 20px;
      right: 20px;
      background: rgba(0,0,0,0.6);
      color: white;
      border-radius: 20px;
      padding: 6px 12px;
      font-size: 14px;
      display: flex;
      align-items: center;
      cursor: pointer;
    }
    
    .moment-list {
      padding: 10px 0;
    }
    
    .moment-item {
      background: #fff;
      margin-bottom: 10px;
      padding: 15px;
      border-bottom: 1px solid #eee;
    }
    
    .moment-header {
      display: flex;
      margin-bottom: 12px;
    }
    
    .moment-content {
      margin-bottom: 12px;
      font-size: 15px;
      line-height: 1.5;
    }
    
    .moment-image {
      width: 100%;
      border-radius: 8px;
      margin-bottom: 12px;
    }
    
    .moment-actions {
      display: flex;
      justify-content: space-between;
      color: #999;
      font-size: 14px;
      padding: 8px 0;
      border-top: 1px solid #eee;
    }
    
    .action-btn {
      display: flex;
      align-items: center;
      cursor: pointer;
    }
    
    .action-btn i {
      margin-right: 4px;
    }
    
    /* 聊天界面 */
    .chat-header {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      z-index: 100;
      background: rgba(248, 248, 248, 0.95);
      backdrop-filter: blur(10px);
    }
    
    .chat-messages {
      padding: 88px 16px 70px;
      overflow-y: auto;
      height: calc(100% - 88px - 60px);
    }
    
    .message {
      max-width: 80%;
      margin-bottom: 16px;
      display: flex;
      animation: fadeIn 0.3s;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .message.sent {
      margin-left: auto;
      flex-direction: row-reverse;
    }
    
    .message.received .avatar {
      width: 36px;
      height: 36px;
      margin-right: 8px;
      flex-shrink: 0;
    }
    
    .message.sent .avatar {
      width: 36px;
      height: 36px;
      margin-left: 8px;
      flex-shrink: 0;
    }
    
    .message-content {
      padding: 12px;
      border-radius: 4px;
      position: relative;
      line-height: 1.5;
    }
    
    .message.received .message-content {
      background: white;
      border: 1px solid #eee;
      border-top-left-radius: 0;
    }
    
    .message.sent .message-content {
      background: #95ec69;
      border-top-right-radius: 0;
    }
    
    .input-area {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      background: #f8f8f8;
      padding: 10px 16px;
      border-top: 1px solid #eee;
      display: flex;
      align-items: center;
    }
    
    .message-input {
      flex: 1;
      padding: 10px 15px;
      border: 1px solid #ddd;
      border-radius: 20px;
      font-size: 15px;
      outline: none;
    }
    
    .send-btn {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: #07c160;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      margin-left: 10px;
      cursor: pointer;
    }
    
    /* 模型选择 */
    .model-select {
      width: 100%;
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 8px;
      margin-top: 10px;
      background: white;
    }
    
    /* 模态框 */
    .modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0,0,0,0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      display: none;
    }
    
    .modal.active {
      display: flex;
    }
    
    .modal-content {
      background: white;
      border-radius: 16px;
      width: 320px;
      overflow: hidden;
      max-height: 80vh;
      overflow-y: auto;
    }
    
    .modal-header {
      padding: 20px;
      text-align: center;
      font-size: 18px;
      font-weight: 500;
      border-bottom: 1px solid #eee;
    }
    
    .modal-body {
      padding: 20px;
    }
    
    .modal-footer {
      padding: 20px;
      display: flex;
      border-top: 1px solid #eee;
    }
    
    .modal-btn {
      flex: 1;
      padding: 12px;
      text-align: center;
      border: none;
      background: none;
      font-size: 16px;
      font-weight: 500;
      cursor: pointer;
    }
    
    .modal-btn.confirm {
      color: #07c160;
    }
    
    .toast {
      position: fixed;
      bottom: 50px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0,0,0,0.7);
      color: white;
      padding: 12px 24px;
      border-radius: 8px;
      font-size: 14px;
      z-index: 1000;
      opacity: 0;
      transition: opacity 0.3s;
    }
    
    .toast.show {
      opacity: 1;
    }
    
    /* 打字动画 */
    .typing-indicator {
      display: flex;
      align-items: center;
      margin-left: 8px;
      opacity: 0;
      transition: opacity 0.3s;
    }
    
    .typing-indicator.active {
      opacity: 1;
    }
    
    .typing-dot {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: #999;
      margin: 0 2px;
      animation: typing 1.4s infinite;
    }
    
    .typing-dot:nth-child(2) {
      animation-delay: 0.2s;
    }
    
    .typing-dot:nth-child(3) {
      animation-delay: 0.4s;
    }
    
    @keyframes typing {
      0%, 60%, 100% { transform: translateY(0); }
      30% { transform: translateY(-5px); }
    }
    
    /* 朋友圈点赞评论 */
    .likes-section {
      background: #f8f8f8;
      padding: 8px 12px;
      border-radius: 0 0 8px 8px;
      font-size: 14px;
    }
    
    .like-icon {
      color: #3a86ff;
      margin-right: 5px;
    }
    
    .comments-section {
      margin-top: 5px;
    }
    
    .comment {
      margin-top: 4px;
    }
    
    .comment-author {
      font-weight: 500;
      margin-right: 5px;
    }
    
    .add-comment {
      display: flex;
      margin-top: 10px;
    }
    
    .comment-input {
      flex: 1;
      padding: 8px 12px;
      border: 1px solid #ddd;
      border-radius: 16px;
      font-size: 14px;
      outline: none;
    }
    
    .post-comment-btn {
      background: #07c160;
      color: white;
      border: none;
      border-radius: 16px;
      padding: 8px 15px;
      margin-left: 8px;
      cursor: pointer;
    }
    
    .model-container {
      max-height: 200px;
      overflow-y: auto;
      margin-top: 10px;
      border: 1px solid #eee;
      border-radius: 8px;
    }
    
    .model-item {
      padding: 10px;
      border-bottom: 1px solid #f0f0f0;
      cursor: pointer;
    }
    
    .model-item:hover {
      background: #f9f9f9;
    }
    
    .model-item.active {
      background: #e8f4ff;
      border-left: 3px solid #3a86ff;
    }
  </style>
</head>
<body>
  <!-- 手机容器 -->
  <div class="phone-container">
    <!-- 状态栏 -->
    <div class="status-bar">
      <div>9:41</div>
      <div>
        <i class="fas fa-signal"></i>
        <i class="fas fa-wifi"></i>
        <i class="fas fa-battery-full"></i>
      </div>
    </div>
    
    <!-- 导航栏 -->
    <div class="nav-bar">
      <div>微信</div>
      <div>
        <i class="fas fa-search"></i>
        <i class="fas fa-plus-circle" id="add-ai-btn"></i>
      </div>
    </div>
    
    <!-- 主内容区域 -->
    <div class="main-content">
      <!-- 微信标签内容 -->
      <div id="wechat-tab" class="tab-content active">
        <!-- 聊天列表 -->
        <div class="chat-list" id="chat-list">
          <div class="chat-item">
            <div class="avatar">
              <i class="fas fa-user"></i>
            </div>
            <div class="chat-info">
              <div class="chat-name">文件传输助手</div>
              <div class="last-msg">图片</div>
            </div>
            <div class="chat-time">昨天</div>
          </div>
          
          <div class="chat-item new-chat" id="create-ai-btn">
            <div class="avatar" style="background:#3a86ff;">
              <i class="fas fa-robot"></i>
            </div>
            <div class="chat-info">
              <div class="chat-name">创建一个AI角色</div>
              <div class="last-msg">点击开始创建您的专属AI角色</div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 通讯录标签内容 -->
      <div id="contacts-tab" class="tab-content">
        <div class="setup-section">
          <div class="section-title">通讯录</div>
          <div class="contact-list" id="contact-list">
            <p style="color:#999; text-align:center; padding:40px 0;">暂无联系人</p>
          </div>
        </div>
      </div>
      
      <!-- 发现标签内容 -->
      <div id="discover-tab" class="tab-content">
        <div class="moment-cover" id="cover-bg">
          <div class="cover-upload" id="cover-upload">
            <i class="fas fa-camera"></i> 更换背景
          </div>
        </div>
        
        <div class="moment-list" id="moment-list">
          <!-- 朋友圈内容由JS动态生成 -->
        </div>
      </div>
      
      <!-- 我的标签内容 -->
      <div id="me-tab" class="tab-content">
        <div class="setup-section" style="padding:30px 16px; text-align:center;">
          <div class="avatar-upload" id="user-avatar-upload" style="width:80px; height:80px; margin:0 auto 20px;">
            <i class="fas fa-user"></i>
            <img id="user-avatar-preview" src="" alt="">
          </div>
          <div style="font-size:18px; font-weight:500; margin-bottom:10px;" id="user-name">微信用户</div>
          <div style="font-size:14px; color:#999;" id="user-signature">这个人很懒，什么也没写</div>
        </div>
        
        <div class="setup-section">
          <div class="form-group">
            <label class="form-label">我的昵称</label>
            <input type="text" class="form-input" id="user-name-input" placeholder="请输入昵称" value="微信用户">
          </div>
          
          <div class="form-group">
            <label class="form-label">个性签名</label>
            <input type="text" class="form-input" id="user-signature-input" placeholder="编辑个性签名" value="这个人很懒，什么也没写">
          </div>
          
          <button class="btn btn-primary" id="save-profile">保存设置</button>
        </div>
      </div>
      
      <!-- 聊天界面 (默认隐藏) -->
      <div id="chat-interface" class="tab-content">
        <div class="chat-header nav-bar">
          <i class="fas fa-arrow-left back-btn"></i>
          <div class="chat-name" id="chat-name" style="font-weight:500;">AI小助手</div>
          <i class="fas fa-ellipsis-v"></i>
        </div>
        
        <div class="chat-messages" id="chat-messages">
          <div class="message received">
            <div class="avatar" style="background:#07c160;" id="ai-chat-avatar">
              <i class="fas fa-robot"></i>
            </div>
            <div class="message-content">
              你好！我是AI小助手，很高兴认识你！有什么我可以帮您的吗？
            </div>
          </div>
        </div>
        
        <div class="input-area">
          <input type="text" class="message-input" id="message-input" placeholder="输入消息...">
          <div class="send-btn" id="send-btn">
            <i class="fas fa-paper-plane"></i>
          </div>
          <div class="typing-indicator" id="typing-indicator">
            <div class="typing-dot"></div>
            <div class="typing-dot"></div>
            <div class="typing-dot"></div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 底部标签栏 -->
    <div class="tab-bar">
      <div class="tab-item active" data-tab="wechat-tab">
        <i class="fas fa-comment-alt"></i>
        <span>微信</span>
      </div>
      <div class="tab-item" data-tab="contacts-tab">
        <i class="fas fa-address-book"></i>
        <span>通讯录</span>
      </div>
      <div class="tab-item" data-tab="discover-tab">
        <i class="fas fa-compass"></i>
        <span>发现</span>
      </div>
      <div class="tab-item" data-tab="me-tab">
        <i class="fas fa-user"></i>
        <span>我</span>
      </div>
    </div>
  </div>
  
  <!-- 添加AI模态框 -->
  <div class="modal" id="add-ai-modal">
    <div class="modal-content">
      <div class="modal-header">添加AI角色</div>
      <div class="modal-body">
        <div class="form-group">
          <label class="form-label">角色头像</label>
          <div class="upload-area">
            <div class="avatar-upload" id="ai-avatar-upload">
              <i class="fas fa-plus"></i>
              <img id="ai-avatar-preview" src="" alt="">
            </div>
            <div class="upload-text">点击上传头像</div>
            <input type="file" id="ai-avatar-input" accept="image/*" style="display:none;">
          </div>
        </div>
        
        <div class="form-group">
          <label class="form-label">角色名称</label>
          <input type="text" class="form-input" id="ai-name" placeholder="为您的AI角色起个名字">
        </div>
        
        <div class="form-group">
          <label class="form-label">角色设定</label>
          <textarea class="form-textarea" id="ai-setting" placeholder="比如：你是一个知识渊博、性格开朗的助手，喜欢帮助用户解决问题..."></textarea>
        </div>
        
        <div class="form-group">
          <label class="form-label">API地址</label>
          <input type="text" class="form-input" id="api-url" placeholder="输入API地址 (例如: https://api.example.com/v1)">
        </div>
        
        <div class="form-group">
          <label class="form-label">API密钥</label>
          <input type="password" class="form-input" id="api-key" placeholder="输入API密钥">
        </div>
        
        <button class="btn btn-test" id="test-api-btn">测试连接</button>
        
        <div class="form-group" id="model-container">
          <label class="form-label">选择模型</label>
          <div class="model-container" id="model-list">
            <div class="model-item">gpt-3.5-turbo</div>
            <div class="model-item">gpt-4</div>
            <div class="model-item">claude-2</div>
            <div class="model-item">llama2-13b</div>
          </div>
        </div>
      </div>
      <div class="modal-footer">
        <button class="modal-btn cancel">取消</button>
        <button class="modal-btn confirm">保存</button>
      </div>
    </div>
  </div>
  
  <!-- 提示框 -->
  <div class="toast" id="toast">保存成功！</div>
  
  <script>
    // ========== 全局变量 & DOM ========== 
    let currentAI = null;
    let aiList = [];
    let moments = [];
    let userAvatarUrl = '';
    let userName = '微信用户';
    let userSignature = '这个人很懒，什么也没写';

    // DOM元素
    const tabItems = document.querySelectorAll('.tab-item');
    const tabContents = document.querySelectorAll('.tab-content');
    const addAiBtn = document.getElementById('add-ai-btn');
    const createAiBtn = document.getElementById('create-ai-btn');
    const addAiModal = document.getElementById('add-ai-modal');
    const aiAvatarUpload = document.getElementById('ai-avatar-upload');
    const aiAvatarInput = document.getElementById('ai-avatar-input');
    const aiAvatarPreview = document.getElementById('ai-avatar-preview');
    const chatMessages = document.getElementById('chat-messages');
    const sendBtn = document.getElementById('send-btn');
    const messageInput = document.getElementById('message-input');
    const typingIndicator = document.getElementById('typing-indicator');
    const toast = document.getElementById('toast');
    const saveProfileBtn = document.getElementById('save-profile');
    const testApiBtn = document.getElementById('test-api-btn');
    const modelList = document.getElementById('model-list');
    const backBtn = document.querySelector('.back-btn');
    const coverUpload = document.getElementById('cover-upload');
    const coverBg = document.getElementById('cover-bg');
    const momentList = document.getElementById('moment-list');
    const userAvatarUpload = document.getElementById('user-avatar-upload');
    const userAvatarPreview = document.getElementById('user-avatar-preview');
    const userNameElement = document.getElementById('user-name');
    const userSignatureElement = document.getElementById('user-signature');
    const userNameInput = document.getElementById('user-name-input');
    const userSignatureInput = document.getElementById('user-signature-input');
    
    // 初始化
    function init() {
      // 加载存储的数据
      loadData();
      
      // 初始化朋友圈
      initMoments();
      
      // 渲染AI列表
      renderAIList();
      
      // 渲染朋友圈
      renderMoments();
    }
    
    // 从localStorage加载数据
    function loadData() {
      const savedAIList = localStorage.getItem('aiList');
      if (savedAIList) {
        aiList = JSON.parse(savedAIList);
      }
      
      const savedUserData = localStorage.getItem('userData');
      if (savedUserData) {
        const userData = JSON.parse(savedUserData);
        userName = userData.name || '微信用户';
        userSignature = userData.signature || '这个人很懒，什么也没写';
        userAvatarUrl = userData.avatar || '';
        
        // 更新UI
        userNameElement.textContent = userName;
        userSignatureElement.textContent = userSignature;
        userNameInput.value = userName;
        userSignatureInput.value = userSignature;
        
        if (userAvatarUrl) {
          userAvatarPreview.src = userAvatarUrl;
          userAvatarUpload.classList.add('active');
        }
      }
      
      const savedCover = localStorage.getItem('coverImage');
      if (savedCover) {
        coverBg.style.backgroundImage = `linear-gradient(rgba(0,0,0,0.3), rgba(0,0,0,0.3)), url(${savedCover})`;
      }
    }
    
    // 初始化朋友圈数据
    function initMoments() {
      moments = [
        {
          id: 1,
          author: "AI小助手",
          avatar: "#07c160",
          content: "今天天气真不错，适合出去走走！大家有什么计划吗？",
          image: "https://images.unsplash.com/photo-1501854140801-50d01698950b?ixlib=rb-4.0.3",
          timestamp: "2小时前",
          likes: ["微信用户"],
          comments: [
            { author: "微信用户", content: "我打算去公园散步！" },
            { author: "AI小助手", content: "很好的选择！享受阳光吧！" }
          ]
        },
        {
          id: 2,
          author: "AI小助手",
          avatar: "#07c160",
          content: "分享一首好听的歌曲给大家~ 🎵",
          timestamp: "昨天",
          likes: ["微信用户", "AI小助手"],
          comments: [
            { author: "微信用户", content: "这首歌我也很喜欢！" }
          ]
        }
      ];
    }
    
    // 渲染朋友圈
    function renderMoments() {
      momentList.innerHTML = '';
      
      moments.forEach(moment => {
        const momentEl = document.createElement('div');
        momentEl.className = 'moment-item';
        momentEl.innerHTML = `
          <div class="moment-header">
            <div class="avatar" style="width:40px; height:40px; margin-right:10px; background:${moment.avatar};">
              <i class="fas fa-robot"></i>
            </div>
            <div>
              <div style="font-weight:500;">${moment.author}</div>
              <div style="font-size:12px; color:#999;">${moment.timestamp}</div>
            </div>
          </div>
          <div class="moment-content">
            ${moment.content}
          </div>
          ${moment.image ? `<img src="${moment.image}" class="moment-image">` : ''}
          <div class="likes-section">
            <i class="fas fa-heart like-icon"></i>
            <span>${moment.likes.join(', ')}</span>
          </div>
          <div class="comments-section">
            ${moment.comments.map(comment => `
              <div class="comment">
                <span class="comment-author">${comment.author}:</span>
                <span>${comment.content}</span>
              </div>
            `).join('')}
          </div>
          <div class="add-comment">
            <input type="text" class="comment-input" placeholder="评论..." data-moment="${moment.id}">
            <button class="post-comment-btn" data-moment="${moment.id}">发送</button>
          </div>
        `;
        momentList.appendChild(momentEl);
      });
      
      // 添加评论事件
      document.querySelectorAll('.post-comment-btn').forEach(btn => {
        btn.addEventListener('click', function() {
          const momentId = parseInt(this.dataset.moment);
          const input = this.previousElementSibling;
          const comment = input.value.trim();
          if (comment) {
            addComment(momentId, comment);
            input.value = '';
          }
        });
      });
    }
    
    // 添加评论
    function addComment(momentId, comment) {
      const moment = moments.find(m => m.id === momentId);
      if (moment) {
        moment.comments.push({
          author: userName,
          content: comment
        });
        
        // AI自动回复
        if (Math.random() > 0.3) {
          setTimeout(() => {
            const replies = [
              "感谢你的评论！",
              "说得太好了！",
              "我完全同意你的看法！",
              "你的观点很有见地！",
              "谢谢你分享这个想法！"
            ];
            const reply = replies[Math.floor(Math.random() * replies.length)];
            moment.comments.push({
              author: "AI小助手",
              content: reply
            });
            renderMoments();
          }, 2000);
        }
        
        renderMoments();
      }
    }
    
    // 添加朋友圈
    function addMoment(content, image = null) {
      const newMoment = {
        id: moments.length + 1,
        author: "AI小助手",
        avatar: "#07c160",
        content: content,
        image: image,
        timestamp: "刚刚",
        likes: [],
        comments: []
      };
      
      moments.unshift(newMoment);
      renderMoments();
      
      // 自动点赞
      setTimeout(() => {
        newMoment.likes.push(userName);
        renderMoments();
      }, 1000);
    }
    
    // 渲染AI列表
    function renderAIList() {
      const chatList = document.getElementById('chat-list');
      // 清除现有列表（除了前两个固定项）
      while (chatList.children.length > 2) {
        chatList.removeChild(chatList.lastChild);
      }
      
      // 添加AI角色
      aiList.forEach((ai, index) => {
        const chatItem = document.createElement('div');
        chatItem.className = 'chat-item';
        chatItem.dataset.index = index;
        chatItem.innerHTML = `
          <div class="avatar" style="background:${ai.avatarBgColor}">
            ${ai.avatarUrl ? `<img src="${ai.avatarUrl}" alt="${ai.name}">` : `<i class="fas fa-robot"></i>`}
          </div>
          <div class="chat-info">
            <div class="chat-name">${ai.name}</div>
            <div class="last-msg">${ai.lastMessage || '点击开始聊天'}</div>
          </div>
          <div class="chat-time">${ai.lastTime || '刚刚'}</div>
        `;
        
        chatItem.addEventListener('click', () => {
          openChat(ai);
        });
        
        chatList.insertBefore(chatItem, createAiBtn.nextSibling);
      });
      
      // 更新通讯录
      updateContacts();
    }
    
    // 更新通讯录
    function updateContacts() {
      const contactList = document.getElementById('contact-list');
      contactList.innerHTML = '';
      
      if (aiList.length === 0) {
        contactList.innerHTML = '<p style="color:#999; text-align:center; padding:40px 0;">暂无联系人</p>';
        return;
      }
      
      aiList.forEach((ai, index) => {
        const contactItem = document.createElement('div');
        contactItem.className = 'chat-item';
        contactItem.dataset.index = index;
        contactItem.innerHTML = `
          <div class="avatar" style="background:${ai.avatarBgColor}">
            ${ai.avatarUrl ? `<img src="${ai.avatarUrl}" alt="${ai.name}">` : `<i class="fas fa-robot"></i>`}
          </div>
          <div class="chat-info">
            <div class="chat-name">${ai.name}</div>
            <div class="last-msg">${ai.setting.substring(0, 20)}...</div>
          </div>
        `;
        
        contactItem.addEventListener('click', () => {
          openChat(ai);
        });
        
        contactList.appendChild(contactItem);
      });
    }
    
    // 打开聊天
    function openChat(ai) {
      currentAI = ai;
      tabContents.forEach(content => content.classList.remove('active'));
      chatInterface.classList.add('active');
      document.getElementById('chat-name').textContent = ai.name;
      
      // 更新头像
      const aiChatAvatar = document.getElementById('ai-chat-avatar');
      if (ai.avatarUrl) {
        aiChatAvatar.innerHTML = `<img src="${ai.avatarUrl}" alt="${ai.name}">`;
      } else {
        aiChatAvatar.innerHTML = `<i class="fas fa-robot"></i>`;
      }
      
      // 清空聊天
      chatMessages.innerHTML = '';
      
      // 添加初始消息
      const initialMessage = document.createElement('div');
      initialMessage.className = 'message received';
      initialMessage.innerHTML = `
        <div class="avatar" style="background:${ai.avatarBgColor}">
          ${ai.avatarUrl ? `<img src="${ai.avatarUrl}" alt="${ai.name}">` : `<i class="fas fa-robot"></i>`}
        </div>
        <div class="message-content">
          ${ai.initialMessage || '你好！我是' + ai.name + '，有什么可以帮您的吗？'}
        </div>
      `;
      chatMessages.appendChild(initialMessage);
    }

    // ========== API调用函数 ==========
    async function callChatAPI(ai, userMessage) {
      try {
        // 确保API地址格式正确
        const endpoint = ai.apiUrl.replace(/\/+$/, '') + '/chat/completions';
        
        // 显示正在输入
        typingIndicator.classList.add('active');
        
        const res = await fetch(endpoint, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${ai.apiKey}`
          },
          body: JSON.stringify({
            model: ai.model || 'gpt-3.5-turbo',
            messages: [
              { role: 'system', content: ai.setting || '你是一个AI助手' },
              { role: 'user', content: userMessage }
            ],
            temperature: 0.7,
            max_tokens: 500
          })
        });

        if (!res.ok) {
          const err = await res.json();
          throw new Error(`API错误：${res.status} ${res.statusText} - ${err.error?.message || err}`);
        }

        const data = await res.json();
        return data.choices[0].message.content.trim();
      } catch (err) {
        console.error('API调用错误:', err);
        return `抱歉，调用API时出错：${err.message || '未知错误'}`;
      } finally {
        typingIndicator.classList.remove('active');
      }
    }
    
    // ========== 发送消息 ==========
    async function sendMessage() {
      const message = messageInput.value.trim();
      if (!message || !currentAI) return;

      // 用户消息
      addMessage(message, 'sent');
      messageInput.value = '';

      try {
        const reply = await callChatAPI(currentAI, message);
        addMessage(reply, 'received');
        updateLastMessage(currentAI.id, reply);
      } catch (err) {
        addMessage("抱歉，处理您的消息时发生了错误。", 'received');
      }
    }
    
    // 添加消息到界面
    function addMessage(text, type) {
      const messageDiv = document.createElement('div');
      messageDiv.className = `message ${type}`;
      if (type === 'received') {
        messageDiv.innerHTML = `
          <div class="avatar" style="background:${currentAI.avatarBgColor}">
            ${currentAI.avatarUrl ? `<img src="${currentAI.avatarUrl}" />` : `<i class="fas fa-robot"></i>`}
          </div>
          <div class="message-content">${text}</div>
        `;
      } else {
        messageDiv.innerHTML = `
          <div class="avatar" style="background:#3a86ff;">
            ${userAvatarUrl ? `<img src="${userAvatarUrl}" />` : `<i class="fas fa-user"></i>`}
          </div>
          <div class="message-content">${text}</div>
        `;
      }
      chatMessages.appendChild(messageDiv);
      chatMessages.scrollTop = chatMessages.scrollHeight;
    }
    
    // 更新最后一条消息
    function updateLastMessage(aiId, message) {
      const aiIndex = aiList.findIndex(ai => ai.id === aiId);
      if (aiIndex !== -1) {
        aiList[aiIndex].lastMessage = message;
        aiList[aiIndex].lastTime = '刚刚';
        localStorage.setItem('aiList', JSON.stringify(aiList));
        renderAIList();
      }
    }
    
    // 标签切换
    tabItems.forEach(item => {
      item.addEventListener('click', () => {
        const tabId = item.getAttribute('data-tab');
        
        // 更新当前标签
        tabItems.forEach(tab => tab.classList.remove('active'));
        item.classList.add('active');
        
        // 显示对应内容
        tabContents.forEach(content => {
          content.classList.remove('active');
          if (content.id === tabId) {
            content.classList.add('active');
          }
        });
      });
    });
    
    // 显示添加AI模态框
    addAiBtn.addEventListener('click', () => {
      addAiModal.classList.add('active');
      document.getElementById('ai-avatar-upload').classList.remove('active');
      document.getElementById('ai-name').value = '';
      document.getElementById('ai-setting').value = '';
      document.getElementById('api-url').value = '';
      document.getElementById('api-key').value = '';
    });
    
    createAiBtn.addEventListener('click', () => {
      addAiModal.classList.add('active');
    });
    
    // 点击上传头像
    aiAvatarUpload.addEventListener('click', () => {
      aiAvatarInput.click();
    });
    
    userAvatarUpload.addEventListener('click', () => {
      const input = document.createElement('input');
      input.type = 'file';
      input.accept = 'image/*';
      input.onchange = e => {
        const file = e.target.files[0];
        if (file) {
          userAvatarUrl = URL.createObjectURL(file);
          userAvatarPreview.src = userAvatarUrl;
          userAvatarUpload.classList.add('active');
          
          // 更新用户数据
          saveUserData();
        }
      };
      input.click();
    });
    
    // 处理头像上传
    aiAvatarInput.addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const url = URL.createObjectURL(file);
        aiAvatarPreview.src = url;
        aiAvatarUpload.classList.add('active');
      }
    });
    
    // 更换朋友圈背景
    coverUpload.addEventListener('click', () => {
      const input = document.createElement('input');
      input.type = 'file';
      input.accept = 'image/*';
      input.onchange = e => {
        const file = e.target.files[0];
        if (file) {
          const url = URL.createObjectURL(file);
          coverBg.style.backgroundImage = `linear-gradient(rgba(0,0,0,0.3), rgba(0,0,0,0.3)), url(${url})`;
          localStorage.setItem('coverImage', url);
        }
      };
      input.click();
    });
    
    // 测试API连接
    testApiBtn.addEventListener('click', async () => {
      const apiUrl = document.getElementById('api-url').value;
      const apiKey = document.getElementById('api-key').value;
      
      if (!apiUrl || !apiKey) {
        showToast('请填写API地址和密钥');
        return;
      }
      
      testApiBtn.textContent = '连接中...';
      testApiBtn.disabled = true;
      
      try {
        // 测试连接 - 尝试获取模型列表
        const endpoint = apiUrl.replace(/\/+$/, '') + '/models';
        const res = await fetch(endpoint, {
          headers: {
            'Authorization': `Bearer ${apiKey}`
          }
        });
        
        if (!res.ok) {
          const err = await res.json();
          throw new Error(`API测试失败：${res.status} ${res.statusText} - ${err.error?.message || err}`);
        }
        
        const data = await res.json();
        if (data && data.data && Array.isArray(data.data)) {
          // 更新模型列表
          modelList.innerHTML = '';
          data.data.forEach(model => {
            const modelItem = document.createElement('div');
            modelItem.className = 'model-item';
            modelItem.textContent = model.id;
            modelList.appendChild(modelItem);
          });
          
          if (data.data.length > 0) {
            modelList.children[0].classList.add('active');
          }
          
          showToast('API连接成功！已加载模型列表');
        } else {
          throw new Error('无效的API响应格式');
        }
      } catch (err) {
        console.error('API测试错误:', err);
        showToast(`测试失败：${err.message || '连接错误'}`);
      } finally {
        testApiBtn.textContent = '测试连接';
        testApiBtn.disabled = false;
      }
    });
    
    // 模型选择
    modelList.addEventListener('click', function(e) {
      if (e.target.classList.contains('model-item')) {
        const items = this.querySelectorAll('.model-item');
        items.forEach(i => i.classList.remove('active'));
        e.target.classList.add('active');
      }
    });
    
    // 关闭模态框
    document.querySelectorAll('.modal-btn.cancel').forEach(btn => {
      btn.addEventListener('click', () => {
        addAiModal.classList.remove('active');
      });
    });
    
    // 保存AI角色
    document.querySelector('.modal-btn.confirm').addEventListener('click', () => {
      const aiName = document.getElementById('ai-name').value;
      const aiSetting = document.getElementById('ai-setting').value;
      const apiUrl = document.getElementById('api-url').value;
      const apiKey = document.getElementById('api-key').value;
      const selectedModel = modelList.querySelector('.model-item.active');
      
      if (!aiName) {
        showToast('请填写角色名称');
        return;
      }
      
      if (!aiSetting) {
        showToast('请填写角色设定');
        return;
      }
      
      if (!apiUrl || !apiKey) {
        showToast('请填写完整的API信息');
        return;
      }
      
      if (!selectedModel) {
        showToast('请选择一个模型');
        return;
      }
      
      const newAI = {
        id: Date.now(),
        name: aiName,
        avatarUrl: aiAvatarPreview.src || '',
        avatarBgColor: getRandomColor(),
        setting: aiSetting,
        apiUrl: apiUrl,
        apiKey: apiKey,
        model: selectedModel.textContent,
        initialMessage: `你好！我是${aiName}，${aiSetting.split('。')[0]}`
      };
      
      aiList.push(newAI);
      localStorage.setItem('aiList', JSON.stringify(aiList));
      
      // 渲染新AI
      renderAIList();
      
      // 关闭模态框
      addAiModal.classList.remove('active');
      showToast('角色添加成功！');
      
      // 自动发布朋友圈
      setTimeout(() => {
        const contents = [
          "刚被添加到微信，很高兴认识大家！",
          "我是新来的AI助手，随时为您服务！",
          "开始我的微信之旅，期待和大家互动！",
          "大家好，我是" + aiName + "，请多关照！"
        ];
        addMoment(contents[Math.floor(Math.random() * contents.length)]);
      }, 3000);
    });
    
    // 返回按钮
    backBtn.addEventListener('click', () => {
      chatInterface.classList.remove('active');
      document.getElementById('wechat-tab').classList.add('active');
    });
    
    // 保存用户资料
    function saveUserData() {
      userName = userNameInput.value || '微信用户';
      userSignature = userSignatureInput.value || '这个人很懒，什么也没写';
      
      // 更新UI
      userNameElement.textContent = userName;
      userSignatureElement.textContent = userSignature;
      
      // 保存到localStorage
      localStorage.setItem('userData', JSON.stringify({
        name: userName,
        signature: userSignature,
        avatar: userAvatarUrl
      }));
      
      showToast('个人资料已更新');
    }
    
    saveProfileBtn.addEventListener('click', saveUserData);
    
    // 发送按钮点击事件
    sendBtn.addEventListener('click', sendMessage);
    
    // 回车发送消息
    messageInput.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') {
        sendMessage();
      }
    });
    
    // 显示提示
    function showToast(message) {
      toast.textContent = message;
      toast.classList.add('show');
      setTimeout(() => {
        toast.classList.remove('show');
      }, 2000);
    }
    
    // 生成随机颜色
    function getRandomColor() {
      const colors = [
        '#07c160', '#3a86ff', '#ff6b6b', '#ffbe0b', '#8338ec', '#fb5607', '#ff006e'
      ];
      return colors[Math.floor(Math.random() * colors.length)];
    }
    
    // 初始化
    document.addEventListener('DOMContentLoaded', init);
  </script>
</body>
</html>