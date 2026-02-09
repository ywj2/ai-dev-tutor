# AI开发代理指南

此文件包含构建/测试命令和代码风格指南，供在仓库中工作的AI代理使用。

## 项目概述

这是一个Next.js 16应用程序，使用TypeScript、Tailwind CSS v4和ESLint配置。项目遵循现代React模式，使用App Router。

## 构建、测试和Lint命令

### 开发
```bash
# 启动开发服务器
npm run dev

# 构建生产版本
npm run build

# 启动生产服务器
npm run start

# 运行代码检查器
npm run lint
```

### 测试
当前项目没有配置测试框架。添加测试时：
- 使用Jest或Vitest进行单元测试
- 使用React Testing Library进行组件测试
- 根据需要将测试脚本添加到package.json

## 代码风格指南

### TypeScript配置
- 目标: ES2017
- 启用严格模式
- 模块解析: bundler
- JSX: react-jsx
- 路径别名: @/* 映射到 ./*

### 导入组织
```typescript
// 1. React/Next.js导入
import { useState, useEffect } from 'react'
import Image from 'next/image'

// 2. 第三方库
import { Button } from '@/components/ui/button'

// 3. 本地导入（使用@别名）
import { getPosts } from '@/lib/api'
import { Layout } from '@/components/layout'
```

### 组件结构
```typescript
// 1. 导入
import React from 'react'

// 2. 类型/接口（如果需要）
interface ComponentProps {
  title: string;
  description?: string;
}

// 3. 组件定义
export default function ComponentName({
  title,
  description = '默认描述'
}: ComponentProps) {
  // 4. Hooks（useState, useEffect等）
  const [state, setState] = useState(null);
  
  // 5. 事件处理函数
  const handleClick = () => {
    setState(!state);
  };
  
  // 6. 渲染
  return (
    <div className="container">
      <h1>{title}</h1>
      {description && <p>{description}</p>}
    </div>
  );
}

// 7. 属性类型（如果不使用TypeScript）
ComponentName.propTypes = {
  title: PropTypes.string.isRequired,
  description: PropTypes.string
};
```

### 命名约定
- **组件**: PascalCase（例如：UserProfile, DataTable）
- **函数/变量**: camelCase（例如：getUserData, isLoading）
- **常量**: UPPER_SNAKE_CASE（例如：API_BASE_URL, MAX_RETRY_ATTEMPTS）
- **文件**: 组件使用PascalCase（例如：UserProfile.tsx），工具使用camelCase（例如：apiClient.ts）
- **CSS类**: Tailwind工具类或BEM方法用于自定义样式

### 错误处理
```typescript
// 使用try-catch处理异步操作
try {
  const data = await fetchUserData();
  setData(data);
} catch (error) {
  console.error('获取数据失败：', error);
  setError('无法加载用户数据');
}

// 为React组件使用适当的错误边界
```

### 状态管理
- 使用React钩子（useState, useEffect, useContext）管理本地状态
- 考虑使用Context API管理全局状态
- 避免直接突变；使用不可变更新
- 优先使用派生状态而非冗余状态

### 样式指南
- 使用Tailwind CSS工具类
- 利用暗黑模式支持（dark:前缀）
- 使用CSS变量进行主题化
- 遵循响应式设计模式（sm:, md:, lg:前缀）

### 文件组织
```
src/
├── components/     # 可重用UI组件
│   ├── ui/         # 基础UI元素
│   └── features/   # 特性特定组件
├── lib/           # 工具函数和助手
├── types/         # TypeScript类型定义
├── hooks/         # 自定义React钩子
├── pages/         # 页面组件（Next.js App Router）
└── styles/        # 全局样式和主题
```

### ESLint配置
- 使用Next.js ESLint配置
- 扩展核心Web vitals和TypeScript规则
- 忽略 .next/、out/、build/ 和 next-env.d.ts

### TypeScript最佳实践
- 为函数使用显式返回类型
- 对象形状优先使用接口而非类型
- 在适当的地方使用泛型类型
- 启用严格模式确保类型安全

### 性能指南
- 对昂贵组件使用React.memo
- 实现适当的加载状态
- 使用Next.js Image组件优化图片
- 使用动态导入进行代码分割

### 安全注意事项
- 为外部链接使用 rel="noopener noreferrer"
- 清理用户输入
- 对敏感数据使用环境变量
- 实现适当的身份验证和授权

## 开发工作流

1. 提交前始终运行 `npm run lint`
2. 使用TypeScript确保类型安全
3. 遵循已建立的组件模式
4. 跨断点测试响应式设计
5. 确保暗黑模式兼容性
6. 使用语义化HTML元素
7. 为Core Web Vitals优化

## 常见模式

### API调用
```typescript
const fetchUsers = async (): Promise<User[]> => {
  try {
    const response = await fetch('/api/users');
    if (!response.ok) {
      throw new Error('网络响应失败');
    }
    return await response.json();
  } catch (error) {
    console.error('获取用户失败：', error);
    throw error;
  }
};
```

### 表单处理
```typescript
const [formData, setFormData] = useState({
  name: '',
  email: ''
});

const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  const { name, value } = e.target;
  setFormData(prev => ({
    ...prev,
    [name]: value
  }));
};
```

### 加载状态
```typescript
const [isLoading, setIsLoading] = useState(false);
const [data, setData] = useState<Data | null>(null);

const loadData = async () => {
  setIsLoading(true);
  try {
    const result = await fetchData();
    setData(result);
  } finally {
    setIsLoading(false);
  }
};
```

## 工具和扩展

- 使用VS Code配合TypeScript和ESLint扩展
- 安装Tailwind CSS IntelliSense获得更好的自动补全
- 使用Prettier进行代码格式化（如果配置）
- 考虑使用Next.js DevTools进行调试