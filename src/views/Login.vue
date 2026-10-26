<template>
  <div class="flex min-h-screen bg-white">
    <!-- 左侧品牌区 -->
    <div
      class="relative hidden w-[46%] flex-col justify-between overflow-hidden bg-gradient-to-br from-emerald-600 via-teal-700 to-teal-900 p-12 text-white lg:flex"
    >
      <!-- 装饰光斑 -->
      <div class="pointer-events-none absolute inset-0" aria-hidden="true">
        <div class="absolute -left-24 -top-24 h-72 w-72 rounded-full bg-white/10 blur-2xl"></div>
        <div class="absolute right-[-80px] top-1/3 h-64 w-64 rounded-full bg-emerald-300/20 blur-3xl"></div>
        <div class="absolute bottom-[-60px] left-1/4 h-56 w-56 rounded-full bg-teal-300/15 blur-3xl"></div>
      </div>

      <div class="relative">
        <AppLogo :size="60" class="rounded-2xl ring-1 ring-white/25" />
        <h1 class="mt-8 text-3xl font-semibold leading-snug">
          校园知识库<br />智能问答系统
        </h1>
        <p class="mt-3 text-sm leading-6 text-emerald-100">
          面向学生与教职工的一站式校园知识问答平台<br />
          教学管理 · 学生事务 · 校园规章 · 后勤服务
        </p>
      </div>

      <ul class="relative space-y-5">
        <li v-for="feature in features" :key="feature.title" class="flex items-start gap-3">
          <span
            class="mt-0.5 inline-flex h-8 w-8 flex-none items-center justify-center rounded-lg bg-white/10 ring-1 ring-white/20"
          >
            <el-icon :size="16"><component :is="feature.icon" /></el-icon>
          </span>
          <span>
            <span class="block text-sm font-medium">{{ feature.title }}</span>
            <span class="block text-xs leading-5 text-emerald-100/80">{{ feature.desc }}</span>
          </span>
        </li>
      </ul>

      <p class="relative text-xs tracking-wide text-emerald-200/60">
        Campus Knowledge RAG · Vue 3 + FastAPI + Milvus
      </p>
    </div>

    <!-- 右侧表单区 -->
    <div class="flex flex-1 items-center justify-center bg-slate-50 p-6">
      <div class="w-full max-w-[400px]">
        <div class="mb-8 flex flex-col items-center text-center lg:items-start lg:text-left">
          <AppLogo :size="48" class="lg:hidden" />
          <h2 class="mt-5 text-2xl font-semibold text-slate-900 lg:mt-0">欢迎登录</h2>
          <p class="mt-1 text-sm text-slate-500">登录后进入校园知识问答工作台</p>
        </div>

        <el-card shadow="never" class="border border-slate-200">
          <el-form
            ref="formRef"
            :model="form"
            :rules="rules"
            label-position="top"
            @keyup.enter="handleLogin"
          >
            <el-form-item label="用户名" prop="username">
              <el-input
                v-model.trim="form.username"
                size="large"
                placeholder="请输入用户名"
                :prefix-icon="User"
                autocomplete="username"
              />
            </el-form-item>

            <el-form-item label="密码" prop="password">
              <el-input
                v-model="form.password"
                type="password"
                size="large"
                placeholder="请输入密码"
                show-password
                :prefix-icon="Lock"
                autocomplete="current-password"
              />
            </el-form-item>

            <el-alert
              v-if="loginError"
              class="mb-4"
              type="error"
              :title="loginError"
              show-icon
              :closable="false"
            />

            <el-button
              type="primary"
              size="large"
              class="w-full mt-2"
              :loading="loading"
              @click="handleLogin"
            >
              登录
            </el-button>
          </el-form>
        </el-card>

        <p class="mt-6 text-center text-xs text-slate-400">
          校园知识库智能问答系统 · 智能问答仅供学习参考
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { ChatDotRound, Collection, Lock, Picture, User } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'
import { useUserStore } from '@/stores/user'
import { getApiErrorMessage } from '@/utils/httpError'
import AppLogo from '@/components/AppLogo.vue'

const route = useRoute()
const router = useRouter()
const userStore = useUserStore()

const features = [
  { icon: ChatDotRound, title: '智能问答', desc: '结合校园知识库上下文精准作答，支持流式输出' },
  { icon: Collection, title: '多知识库', desc: '教学管理、学生事务等资料分区隔离检索' },
  { icon: Picture, title: '图片理解', desc: '支持截图、图片附件提问，识别图中信息' },
]

const formRef = ref(null)
const loading = ref(false)
const loginError = ref('')
const form = reactive({
  username: '',
  password: '',
})

const rules = {
  username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
    { min: 6, message: '密码至少 6 位', trigger: 'blur' },
  ],
}

async function handleLogin() {
  loginError.value = ''
  const valid = await formRef.value?.validate().catch(() => false)
  if (!valid || loading.value) return

  loading.value = true
  try {
    await userStore.login(form)
    ElMessage.success('登录成功')
    router.replace(route.query.redirect || '/chat')
  } catch (error) {
    loginError.value = getApiErrorMessage(error, '用户名或密码错误')
    ElMessage.error(loginError.value)
  } finally {
    loading.value = false
  }
}
</script>
