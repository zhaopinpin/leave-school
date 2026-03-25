<template>
  <view class="page">
    <view class="header">
      <text class="title">智能体 API 调试页</text>
      <text class="subtitle">你只需要填接口地址和密钥即可开始对话</text>
    </view>

    <view class="card">
      <view class="field">
        <text class="label">接口地址</text>
        <input
          class="input"
          v-model="apiBase"
          placeholder="例如：https://api.openai.com/v1/chat/completions"
        />
      </view>

      <view class="field">
        <text class="label">API Key</text>
        <input class="input" v-model="apiKey" password placeholder="输入你的密钥" />
      </view>

      <view class="field">
        <text class="label">模型名</text>
        <input class="input" v-model="model" placeholder="例如：gpt-4o-mini" />
      </view>
    </view>

    <view class="card chat-box">
      <scroll-view scroll-y class="messages" :scroll-top="scrollTop">
        <view v-for="(item, idx) in messages" :key="idx" class="msg" :class="item.role">
          <text class="role">{{ item.role === 'user' ? '你' : '智能体' }}</text>
          <text class="content">{{ item.content }}</text>
        </view>
      </scroll-view>

      <view class="composer">
        <textarea
          class="textarea"
          v-model="inputText"
          placeholder="输入你的问题..."
          auto-height
          maxlength="-1"
        />
        <button class="send" :disabled="loading" @click="sendMessage">
          {{ loading ? '发送中...' : '发送' }}
        </button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      apiBase: 'https://api.openai.com/v1/chat/completions',
      apiKey: '',
      model: 'gpt-4o-mini',
      inputText: '',
      loading: false,
      messages: [
        {
          role: 'assistant',
          content: '你好！配置好 API 后，我们就可以开始聊天。'
        }
      ],
      scrollTop: 0
    };
  },
  methods: {
    async sendMessage() {
      const text = (this.inputText || '').trim();
      if (!this.apiBase || !this.apiKey || !this.model) {
        uni.showToast({
          title: '请先填写接口地址、密钥和模型名',
          icon: 'none'
        });
        return;
      }
      if (!text || this.loading) return;

      this.messages.push({ role: 'user', content: text });
      this.inputText = '';
      this.scrollToBottom();
      this.loading = true;

      try {
        const requestMessages = this.messages.map((m) => ({
          role: m.role === 'assistant' ? 'assistant' : 'user',
          content: m.content
        }));

        const [err, res] = await uni.request({
          url: this.apiBase,
          method: 'POST',
          header: {
            'Content-Type': 'application/json',
            Authorization: `Bearer ${this.apiKey}`
          },
          data: {
            model: this.model,
            messages: requestMessages
          }
        });

        if (err) throw err;

        const reply =
          res &&
          res.data &&
          res.data.choices &&
          res.data.choices[0] &&
          res.data.choices[0].message &&
          res.data.choices[0].message.content;

        if (!reply) {
          const fallback =
            (res && res.data && JSON.stringify(res.data)) || '接口未返回可识别内容';
          this.messages.push({ role: 'assistant', content: `返回异常：${fallback}` });
        } else {
          this.messages.push({ role: 'assistant', content: reply });
        }
      } catch (e) {
        this.messages.push({
          role: 'assistant',
          content: `请求失败：${(e && e.errMsg) || e.message || '未知错误'}`
        });
      } finally {
        this.loading = false;
        this.scrollToBottom();
      }
    },
    scrollToBottom() {
      this.$nextTick(() => {
        this.scrollTop = this.messages.length * 999;
      });
    }
  }
};
</script>

<style>
.page {
  min-height: 100vh;
  background: #f5f7fb;
  padding: 24rpx;
  box-sizing: border-box;
}

.header {
  margin-bottom: 20rpx;
}

.title {
  display: block;
  font-size: 38rpx;
  font-weight: 700;
  color: #1f2937;
}

.subtitle {
  display: block;
  margin-top: 8rpx;
  color: #6b7280;
  font-size: 24rpx;
}

.card {
  background: #fff;
  border-radius: 18rpx;
  padding: 20rpx;
  margin-bottom: 20rpx;
}

.field {
  margin-bottom: 16rpx;
}

.field:last-child {
  margin-bottom: 0;
}

.label {
  display: block;
  font-size: 24rpx;
  color: #374151;
  margin-bottom: 8rpx;
}

.input {
  width: 100%;
  background: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 12rpx;
  padding: 18rpx;
  box-sizing: border-box;
  font-size: 26rpx;
}

.chat-box {
  height: calc(100vh - 420rpx);
  display: flex;
  flex-direction: column;
}

.messages {
  flex: 1;
  padding-bottom: 12rpx;
}

.msg {
  margin-bottom: 16rpx;
  padding: 14rpx;
  border-radius: 12rpx;
  white-space: pre-wrap;
  display: flex;
  flex-direction: column;
}

.msg.user {
  background: #e8f0ff;
}

.msg.assistant {
  background: #f3f4f6;
}

.role {
  font-size: 22rpx;
  color: #6b7280;
  margin-bottom: 6rpx;
}

.content {
  font-size: 27rpx;
  color: #111827;
  line-height: 1.5;
}

.composer {
  border-top: 1px solid #e5e7eb;
  padding-top: 16rpx;
}

.textarea {
  width: 100%;
  min-height: 120rpx;
  max-height: 300rpx;
  background: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 12rpx;
  padding: 16rpx;
  box-sizing: border-box;
  font-size: 26rpx;
}

.send {
  margin-top: 14rpx;
  background: #2563eb;
  color: #fff;
  border-radius: 12rpx;
  font-size: 26rpx;
}
</style>
