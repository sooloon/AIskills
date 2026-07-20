# 研发转化 Skill：HTML 演示界面 → Vue 生产组件

## 1. Skill 目标

将「平台投标演示界面生成 Skill」输出的 **离线 HTML 文件**，自动转化为符合 **WiNEX-BI 开发规范** 的 **Vue 2.7 生产级组件**。

转化结果必须满足：

- 输出 `.vue` 单文件组件，`<script setup lang="ts">` + `<style lang="scss" scoped>`
- 使用 Element UI 2.15.13 组件替代原生 HTML 元素
- 所有样式使用 SCSS scoped + Tailwind CSS
- 所有逻辑使用 TypeScript，禁止 `any` 类型
- 包含完整的 Mock 数据，前后端可并行开发
- 文件命名：kebab-case
- 组件引用：PascalCase
- 可直接放入现有 WiNEX-BI 项目中使用

**触发词**：转化代码、HTML转Vue、研发转化、生产代码生成

---

## 2. 转化工作流

```
输入：离线 HTML 文件（来自投标演示界面生成Skill）
  │
  ├── 步骤1：页面结构分析
  │   ├── 识别页面类型（列表页 / 表单页 / 详情页 / 大屏页 / 配置页）
  │   ├── 识别可拆分组件（表格、卡片、筛选区、弹窗、抽屉、图表）
  │   └── 识别数据流向（父子组件关系、事件传递）
  │
  ├── 步骤2：HTML → Vue Template 转化
  │   ├── 替换原生 HTML 元素为 Element UI 组件
  │   ├── 替换内联事件为 Vue 指令（@click、v-model、v-if、v-for）
  │   └── 替换静态数据绑定为 Vue 响应式绑定
  │
  ├── 步骤3：CSS → SCSS + Tailwind 转化
  │   ├── 提取公共样式变量到 SCSS 变量文件
  │   ├── 替换为 Tailwind 工具类（优先）
  │   ├── 剩余样式转为 SCSS scoped
  │   └── 保持原版视觉风格不变
  │
  ├── 步骤4：JavaScript → TypeScript setup 转化
  │   ├── 声明式 DOM 操作 → 响应式数据驱动
  │   ├── 全局变量 → ref / reactive
  │   ├── 函数 → setup 内方法
  │   ├── 事件监听 → @事件绑定
  │   └── 添加完整 TypeScript 类型定义
  │
  ├── 步骤5：数据 Mock 化
  │   ├── 提取演示数据为独立 mock 文件
  │   ├── 添加 API 接口调用占位
  │   └── 标记数据源切换点（mock → 真实接口）
  │
  └── 输出：Vue 组件文件
      ├── src/views/{module}/*.vue        # 页面组件
      ├── src/components/{module}/*.vue   # 可复用组件
      ├── src/mock/{module}.ts            # Mock 数据
      └── src/api/{module}.ts             # API 接口定义
```

---

## 3. HTML 元素 → Element UI 组件映射表

### 3.1 布局容器

| 原始 HTML 模式 | Element UI 组件 |
|---------------|----------------|
| `<div class="header">` + 标题 | `<el-header>` 或页面标题区 |
| `<div class="sidebar">` + 菜单 | `<el-menu>` + `<el-submenu>` + `<el-menu-item>` |
| `<div class="filter-bar">` | `<el-form :inline="true">` |
| `<div class="card">` | `<el-card>` |
| `<div class="tab-bar">` | `<el-tabs>` + `<el-tab-pane>` |
| `<div class="pagination">` | `<el-pagination>` |

### 3.2 表单控件

| 原始 HTML 模式 | Element UI 组件 |
|---------------|----------------|
| `<input type="text">` | `<el-input>` |
| `<input type="password">` | `<el-input type="password" show-password>` |
| `<input type="number">` | `<el-input-number>` |
| `<input type="textarea">` | `<el-input type="textarea">` |
| `<select>` + `<option>` | `<el-select>` + `<el-option>` |
| `<input type="checkbox">` | `<el-checkbox>` 或 `<el-checkbox-group>` |
| `<input type="radio">` | `<el-radio>` 或 `<el-radio-group>` |
| `<input type="date">` | `<el-date-picker>` |
| `<input type="datetime-local">` | `<el-date-picker type="datetime">` |
| `<input type="file">` | `<el-upload>` |
| `<input type="switch">` | `<el-switch>` |
| `<textarea>` | `<el-input type="textarea">` |

### 3.3 数据展示

| 原始 HTML 模式 | Element UI 组件 |
|---------------|----------------|
| `<table>` + `<thead>` + `<tbody>` | `<el-table>` + `<el-table-column>` |
| 表格操作列 | `<el-table-column>` + `<template #default="scope">` |
| 表格多选列 | `<el-table-column type="selection">` |
| 表格序号列 | `<el-table-column type="index">` |
| `<ul>` / `<ol>` 列表 | `<el-timeline>` 或 `<div>` 按场景 |
| `<dl>` + `<dt>` + `<dd>` 描述列表 | `<el-descriptions>` + `<el-descriptions-item>` |
| 标签/徽章 | `<el-tag>` 或 `<el-badge>` |
| 进度条 | `<el-progress>` |
| 头像 | `<el-avatar>` |
| 树形结构 | `<el-tree>` |
| 级联选择 | `<el-cascader>` |

### 3.4 反馈组件

| 原始 HTML 模式 | Element UI 组件 |
|---------------|----------------|
| `<div class="modal">` / `<div class="dialog">` | `<el-dialog>` |
| `<div class="drawer">` | `<el-drawer>` |
| `<div class="tooltip">` | `<el-tooltip>` |
| `<div class="popover">` | `<el-popover>` |
| `<div class="dropdown">` | `<el-dropdown>` + `<el-dropdown-menu>` |
| `<div class="loading">` | `<el-loading>` 或 `v-loading` 指令 |
| `<div class="empty">` | `<el-empty>` |
| `<div class="message">` | `this.$message()` 或 `ElMessage` |
| `<div class="notification">` | `this.$notify()` 或 `ElNotification` |
| `<div class="confirm">` | `this.$confirm()` 或 `ElMessageBox.confirm` |

### 3.5 按钮

| 原始 HTML 模式 | Element UI 组件 |
|---------------|----------------|
| `<button class="primary">` | `<el-button type="primary">` |
| `<button class="success">` | `<el-button type="success">` |
| `<button class="warning">` | `<el-button type="warning">` |
| `<button class="danger">` | `<el-button type="danger">` |
| `<button class="info">` | `<el-button type="info">` |
| `<button class="text">` | `<el-button type="text">` |
| `<button disabled>` | `<el-button :disabled="true">` |
| `<button class="icon">` | `<el-button icon="el-icon-xxx">` |
| 按钮组 | `<el-button-group>` |

### 3.6 图表

| 原始实现 | 转化方案 |
|---------|---------|
| 原生 Canvas 图表 | 替换为 ECharts 5.3.0，封装为独立 `BaseChart.vue` |
| 原生 SVG 图表 | 同上 |
| CSS 模拟图表 | 同上 |
| 地图 + 飞线 | ECharts + 地图 GeoJSON 数据，封装为 `MapChart.vue` |

### 3.7 导航

| 原始 HTML 模式 | Element UI 组件 |
|---------------|----------------|
| 顶部导航栏 | `<el-menu mode="horizontal">` |
| 面包屑 | `<el-breadcrumb>` + `<el-breadcrumb-item>` |
| 步骤条 | `<el-steps>` + `<el-step>` |
| 页签切换 | `<el-tabs>` + `<el-tab-pane>` |
| 回到顶部 | `<el-backtop>` |

---

## 4. 样式转换规则

### 4.1 SCSS 变量提取

从原始 HTML 的 `:root` 或重复色值中提取 SCSS 变量：

```scss
// src/styles/variables.scss
$color-primary: #176BFF;
$color-primary-light: #00A3FF;
$color-success: #21B573;
$color-warning: #F5A623;
$color-danger: #E5484D;
$color-text-primary: #1F2A3D;
$color-text-regular: #3D4A5C;
$color-text-secondary: #7A8799;
$color-border: #E5EAF2;
$color-bg: #F5F7FB;
$color-white: #FFFFFF;
$radius-sm: 4px;
$radius-md: 6px;
$radius-lg: 8px;
$radius-xl: 10px;
$shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.06);
$shadow-md: 0 2px 8px rgba(0, 0, 0, 0.08);
```

### 4.2 Tailwind CSS 优先

能用 Tailwind 工具类的，优先用 Tailwind：

| 原始 CSS | Tailwind 类 |
|----------|------------|
| `display: flex` | `flex` |
| `justify-content: space-between` | `justify-between` |
| `align-items: center` | `items-center` |
| `padding: 16px` | `p-4` |
| `margin: 8px 12px` | `mx-3 my-2` |
| `width: 100%` | `w-full` |
| `font-size: 14px` | `text-sm` |
| `color: #3D4A5C` | `text-[#3D4A5C]` |
| `background: #F5F7FB` | `bg-[#F5F7FB]` |
| `border-radius: 6px` | `rounded-md` |
| `gap: 12px` | `gap-3` |

### 4.3 SCSS scoped 保留规则

以下情况使用 SCSS scoped：

- Element UI 组件的深度样式覆盖（`:deep()`）
- 复杂动画和过渡效果
- 图表容器样式
- 大屏页面特定布局
- 第三方组件样式覆盖

```scss
<style lang="scss" scoped>
.filter-bar {
    @apply flex items-center gap-3;

    :deep(.el-form-item) {
        margin-bottom: 0;
    }

    :deep(.el-input) {
        width: 200px;
    }
}

.table-card {
    @apply bg-white rounded-lg p-4;
    box-shadow: $shadow-sm;
}
</style>
```

### 4.4 视觉保真原则

转化时必须保持原版 HTML 的视觉风格：

- 配色方案一致（主色、辅色、背景色、文字色）
- 圆角大小一致
- 间距关系一致
- 字体大小层级一致
- 大屏深色风格 vs PC 端浅色风格保持不变
- 卡片阴影关系保持一致

---

## 5. JavaScript → TypeScript 转化规则

### 5.1 数据声明转化

| 原始 JS（HTML 中） | Vue TypeScript |
|-------------------|---------------|
| `let tableData = [...]` | `const tableData = ref<Item[]>([])` |
| `let formData = {name: ''}` | `const formData = reactive<FormData>({name: ''})` |
| `let loading = false` | `const loading = ref(false)` |
| `let currentPage = 1` | `const currentPage = ref(1)` |
| `let total = 0` | `const total = ref(0)` |
| 全局变量 | `provide` / `inject` 或 pinia store |

### 5.2 事件处理转化

| 原始 JS | Vue TypeScript |
|---------|---------------|
| `onclick="handleClick()"` | `@click="handleClick"` |
| `onchange="handleChange(this)"` | `@change="handleChange"` |
| `oninput="handleInput(event)"` | `@input="handleInput"` |
| `onsubmit="handleSubmit(event)"` | `@submit.prevent="handleSubmit"` |
| `onkeyup="handleKeyup(event)"` | `@keyup="handleKeyup"` |
| `document.getElementById('xxx')` | `const xxxRef = ref()` + `ref="xxxRef"` |
| `element.addEventListener(...)` | `@事件` 或 `onMounted` 内注册 |

### 5.3 DOM 操作转化

| 原始 JS DOM 操作 | Vue 响应式驱动 |
|-----------------|---------------|
| `element.style.display = 'none'` | `v-show="visible"` 或 `v-if="visible"` |
| `element.classList.add('active')` | `:class="{ active: isActive }"` |
| `element.innerHTML = 'xxx'` | `{{ text }}` 或 `v-html="htmlContent"` |
| `element.setAttribute('disabled', true)` | `:disabled="isDisabled"` |
| `document.querySelector('.modal').show()` | `dialogVisible.value = true` |
| `document.createElement(...)` | `v-for` 渲染列表 |

### 5.4 条件与循环转化

| 原始 JS | Vue |
|---------|-----|
| `if (condition) { show element }` | `v-if="condition"` |
| `if (condition) { show A } else { show B }` | `v-if` / `v-else` |
| `for (let item of list) { render row }` | `v-for="item in list" :key="item.id"` |
| `switch (type) { case x: ... }` | `v-if` / `v-else-if` 链或 `computed` 动态组件 |

### 5.5 API 调用转化

```typescript
// 原始 HTML 中：直接使用内联数据
// 转化后：定义 API 接口

// src/api/indicator.ts
import request from "@/utils/request";

export interface IndicatorItem {
    id: string;
    code: string;
    name: string;
    domain: string;
    caliber: string;
    dataSource: string;
    status: number;
    createTime: string;
}

export interface IndicatorQuery {
    name?: string;
    domain?: string;
    pageNum: number;
    pageSize: number;
}

export interface PageResult<T> {
    list: T[];
    total: number;
}

export function getIndicatorList(params: IndicatorQuery): Promise<PageResult<IndicatorItem>> {
    return request({
        url: "/api/indicator/list",
        method: "get",
        params,
    });
}

export function createIndicator(data: Partial<IndicatorItem>): Promise<void> {
    return request({
        url: "/api/indicator/create",
        method: "post",
        data,
    });
}

export function updateIndicator(data: IndicatorItem): Promise<void> {
    return request({
        url: "/api/indicator/update",
        method: "put",
        data,
    });
}

export function deleteIndicator(id: string): Promise<void> {
    return request({
        url: `/api/indicator/delete/${id}`,
        method: "delete",
    });
}
```

### 5.6 类型定义规范

所有数据结构必须定义 interface：

```typescript
// 页面状态类型
interface PageState {
    dataList: IndicatorItem[];
    loading: boolean;
    total: number;
}

// 查询参数类型
interface QueryParams {
    name: string;
    domain: string;
    pageNum: number;
    pageSize: number;
}

// 表单数据类型
interface FormData {
    id?: string;
    code: string;
    name: string;
    domain: string;
    caliber: string;
    dataSource: string;
    status: number;
}

// 禁止使用 any
// ❌ const data = ref<any>([])
// ✅ const data = ref<IndicatorItem[]>([])
```

---

## 6. 组件拆分规则

### 6.1 拆分原则

根据原始 HTML 的视觉边界和功能边界拆分组件：

| 拆分标准 | 示例 |
|---------|-----|
| 复用性：同一结构出现 2 次以上 | 卡片列表项、表格操作列 |
| 独立性：功能明确且可独立测试 | 查询筛选区、图表模块 |
| 复杂度：单文件超过 300 行 | 大屏页面拆分为多个面板组件 |
| 职责单一：一个组件只做一件事 | 数据表格 ≠ 数据表格 + 弹窗表单 |

### 6.2 标准组件拆分模式

**列表页拆分**：

```
src/views/indicator/
├── index.vue                    # 主页面：组装所有子组件
├── components/
│   ├── FilterBar.vue           # 查询筛选区
│   ├── DataTable.vue           # 数据表格
│   ├── EditDialog.vue          # 新增/编辑弹窗
│   └── DetailDrawer.vue        # 详情抽屉
```

**大屏页拆分**：

```
src/views/dashboard/
├── index.vue                    # 主页面：网格布局 + 数据驱动
├── components/
│   ├── HeaderPanel.vue         # 顶部标题栏
│   ├── StatCard.vue            # 指标卡片（复用）
│   ├── TrendChart.vue          # 趋势折线图
│   ├── RankChart.vue           # 排名柱状图
│   ├── PieChart.vue            # 结构饼图
│   ├── MapChart.vue            # 地图飞线
│   └── DetailTable.vue         # 明细表格
```

### 6.3 Props 与 Emits 定义

```vue
<!-- FilterBar.vue -->
<script setup lang="ts">
interface Props {
    modelValue: QueryParams;
    domainOptions: SelectOption[];
}

interface Emits {
    (e: "update:modelValue", value: QueryParams): void;
    (e: "search"): void;
    (e: "reset"): void;
}

const props = withDefaults(defineProps<Props>(), {
    domainOptions: () => [],
});

const emit = defineEmits<Emits>();
</script>
```

---

## 7. 图表组件转化规范

### 7.1 图表基础封装

```vue
<!-- src/components/BaseChart.vue -->
<template>
    <div ref="chartRef" class="base-chart w-full h-full"></div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, watch } from "vue";
import * as echarts from "echarts";

interface Props {
    option: echarts.EChartsOption;
    theme?: "light" | "dark";
}

const props = withDefaults(defineProps<Props>(), {
    theme: "light",
});

const chartRef = ref<HTMLDivElement>();
let chartInstance: echarts.ECharts | null = null;

const initChart = () => {
    if (!chartRef.value) return;
    chartInstance = echarts.init(chartRef.value, props.theme);
    chartInstance.setOption(props.option);
};

const resizeChart = () => {
    chartInstance?.resize();
};

watch(
    () => props.option,
    (newOption) => {
        chartInstance?.setOption(newOption, true);
    },
    { deep: true }
);

onMounted(() => {
    initChart();
    window.addEventListener("resize", resizeChart);
});

onBeforeUnmount(() => {
    window.removeEventListener("resize", resizeChart);
    chartInstance?.dispose();
});
</script>

<style lang="scss" scoped>
.base-chart {
    min-height: 200px;
}
</style>
```

### 7.2 原版 HTML 图表数据提取

从原版 HTML JS 中提取图表配置：

- `labels` / `categories` → ECharts `xAxis.data`
- `values` / `series` → ECharts `series[].data`
- `colors` → ECharts `color` 数组
- `title` → ECharts `title.text`
- `legend` → ECharts `legend.data`

---

## 8. Mock 数据生成规范

### 8.1 Mock 文件结构

```typescript
// src/mock/indicator.ts
import type { IndicatorItem, PageResult } from "@/api/indicator";

export const mockIndicatorList: IndicatorItem[] = [
    {
        id: "1",
        code: "IND-GATEWAY-001",
        name: "门急诊人次",
        domain: "医疗服务",
        caliber: "COUNT(DISTINCT VISIT_ID)",
        dataSource: "HIS门诊就诊视图",
        status: 1,
        createTime: "2026-01-15 10:30:00",
    },
    // ... 至少 20 条数据，覆盖分页、筛选场景
];

export function mockGetIndicatorList(params: {
    name?: string;
    domain?: string;
    pageNum: number;
    pageSize: number;
}): Promise<PageResult<IndicatorItem>> {
    return new Promise((resolve) => {
        setTimeout(() => {
            let list = [...mockIndicatorList];

            if (params.name) {
                list = list.filter((item) =>
                    item.name.includes(params.name!)
                );
            }
            if (params.domain) {
                list = list.filter((item) => item.domain === params.domain);
            }

            const total = list.length;
            const start = (params.pageNum - 1) * params.pageSize;
            const pageData = list.slice(start, start + params.pageSize);

            resolve({ list: pageData, total });
        }, 300); // 模拟网络延迟
    });
}
```

### 8.2 数据量要求

Mock 数据必须满足：

- 至少 20 条记录（测试分页）
- 覆盖各种筛选条件下的不同结果
- 包含边界值（空值、最大值、长文本）
- 状态枚举值覆盖所有情况
- 下拉选项数据至少 5 个

### 8.3 接口切换标记

在组件中明确标记 Mock 和真实接口的切换点：

```typescript
// TODO: 对接后端接口时替换
const USE_MOCK = true;

const fetchData = async () => {
    loading.value = true;
    try {
        if (USE_MOCK) {
            const { getIndicatorList } = await import("@/mock/indicator");
            const result = await getIndicatorList(queryParams.value);
            dataList.value = result.list;
            total.value = result.total;
        } else {
            const result = await getIndicatorList(queryParams.value);
            dataList.value = result.list;
            total.value = result.total;
        }
    } finally {
        loading.value = false;
    }
};
```

---

## 9. 完整转化示例

### 9.1 输入：原始 HTML 片段

```html
<div class="filter-bar">
    <input type="text" class="search-input" placeholder="请输入指标名称" oninput="handleSearch(this.value)">
    <select class="domain-select" onchange="handleDomainChange(this.value)">
        <option value="">全部业务域</option>
        <option value="医疗服务">医疗服务</option>
        <option value="公卫服务">公卫服务</option>
    </select>
    <button class="btn-primary" onclick="handleSearch()">查询</button>
    <button class="btn-default" onclick="handleReset()">重置</button>
    <button class="btn-success" onclick="handleAdd()">新增</button>
</div>

<table class="data-table">
    <thead>
        <tr>
            <th>指标编码</th>
            <th>指标名称</th>
            <th>业务域</th>
            <th>状态</th>
            <th>创建时间</th>
            <th>操作</th>
        </tr>
    </thead>
    <tbody id="tableBody"></tbody>
</table>

<div class="pagination">
    <span>共 <strong id="totalCount">0</strong> 条</span>
</div>
```

### 9.2 输出：Vue 组件

```vue
<template>
    <div class="indicator-management">
        <!-- 筛选区 -->
        <div class="filter-bar">
            <el-form :inline="true" :model="queryParams" class="filter-form">
                <el-form-item label="指标名称">
                    <el-input
                        v-model="queryParams.name"
                        placeholder="请输入指标名称"
                        clearable
                        @clear="handleSearch"
                    />
                </el-form-item>
                <el-form-item label="业务域">
                    <el-select
                        v-model="queryParams.domain"
                        placeholder="全部业务域"
                        clearable
                        @change="handleSearch"
                    >
                        <el-option
                            v-for="item in domainOptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value"
                        />
                    </el-select>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="handleSearch">查询</el-button>
                    <el-button @click="handleReset">重置</el-button>
                    <el-button type="success" @click="handleAdd">新增</el-button>
                </el-form-item>
            </el-form>
        </div>

        <!-- 数据表格 -->
        <div class="table-card">
            <el-table
                v-loading="loading"
                :data="dataList"
                border
                stripe
                style="width: 100%"
            >
                <el-table-column
                    prop="code"
                    label="指标编码"
                    min-width="160"
                    show-overflow-tooltip
                />
                <el-table-column
                    prop="name"
                    label="指标名称"
                    min-width="160"
                    show-overflow-tooltip
                />
                <el-table-column
                    prop="domain"
                    label="业务域"
                    width="120"
                />
                <el-table-column
                    prop="status"
                    label="状态"
                    width="100"
                >
                    <template #default="{ row }">
                        <el-tag
                            :type="row.status === 1 ? 'success' : 'info'"
                            size="small"
                        >
                            {{ row.status === 1 ? '启用' : '停用' }}
                        </el-tag>
                    </template>
                </el-table-column>
                <el-table-column
                    prop="createTime"
                    label="创建时间"
                    width="180"
                />
                <el-table-column
                    label="操作"
                    width="180"
                    fixed="right"
                >
                    <template #default="{ row }">
                        <el-button
                            type="text"
                            size="small"
                            @click="handleEdit(row)"
                        >
                            编辑
                        </el-button>
                        <el-button
                            type="text"
                            size="small"
                            @click="handleDetail(row)"
                        >
                            详情
                        </el-button>
                        <el-button
                            type="text"
                            size="small"
                            style="color: #E5484D"
                            @click="handleDelete(row)"
                        >
                            删除
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>
        </div>

        <!-- 分页 -->
        <div class="pagination-bar">
            <el-pagination
                v-model:current-page="queryParams.pageNum"
                v-model:page-size="queryParams.pageSize"
                :total="total"
                :page-sizes="[10, 20, 50, 100]"
                layout="total, sizes, prev, pager, next, jumper"
                @size-change="handleSearch"
                @current-change="handleSearch"
            />
        </div>

        <!-- 编辑弹窗 -->
        <EditDialog
            v-model:visible="editDialogVisible"
            :form-data="currentRow"
            :domain-options="domainOptions"
            @success="handleSearch"
        />
    </div>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted } from "vue";
import { ElMessage, ElMessageBox } from "element-ui";
import EditDialog from "./components/EditDialog.vue";
import type { IndicatorItem, IndicatorQuery } from "@/api/indicator";

interface SelectOption {
    label: string;
    value: string;
}

const USE_MOCK = true;

const loading = ref(false);
const dataList = ref<IndicatorItem[]>([]);
const total = ref(0);
const editDialogVisible = ref(false);
const currentRow = ref<IndicatorItem | null>(null);

const queryParams = reactive<IndicatorQuery>({
    name: "",
    domain: "",
    pageNum: 1,
    pageSize: 20,
});

const domainOptions: SelectOption[] = [
    { label: "医疗服务", value: "医疗服务" },
    { label: "公卫服务", value: "公卫服务" },
    { label: "药品管理", value: "药品管理" },
    { label: "医保结算", value: "医保结算" },
    { label: "运营管理", value: "运营管理" },
];

const fetchData = async () => {
    loading.value = true;
    try {
        if (USE_MOCK) {
            const { mockGetIndicatorList } = await import("@/mock/indicator");
            const result = await mockGetIndicatorList({ ...queryParams });
            dataList.value = result.list;
            total.value = result.total;
        } else {
            const { getIndicatorList } = await import("@/api/indicator");
            const result = await getIndicatorList({ ...queryParams });
            dataList.value = result.list;
            total.value = result.total;
        }
    } finally {
        loading.value = false;
    }
};

const handleSearch = () => {
    queryParams.pageNum = 1;
    fetchData();
};

const handleReset = () => {
    queryParams.name = "";
    queryParams.domain = "";
    handleSearch();
};

const handleAdd = () => {
    currentRow.value = null;
    editDialogVisible.value = true;
};

const handleEdit = (row: IndicatorItem) => {
    currentRow.value = { ...row };
    editDialogVisible.value = true;
};

const handleDetail = (row: IndicatorItem) => {
    // TODO: 详情抽屉或跳转
    console.log("查看详情", row);
};

const handleDelete = async (row: IndicatorItem) => {
    try {
        await ElMessageBox.confirm(
            `确认删除指标「${row.name}」吗？`,
            "删除确认",
            {
                confirmButtonText: "确定",
                cancelButtonText: "取消",
                type: "warning",
            }
        );
        if (USE_MOCK) {
            ElMessage.success("删除成功");
            fetchData();
        } else {
            const { deleteIndicator } = await import("@/api/indicator");
            await deleteIndicator(row.id);
            ElMessage.success("删除成功");
            fetchData();
        }
    } catch {
        // 取消删除
    }
};

onMounted(() => {
    fetchData();
});
</script>

<style lang="scss" scoped>
.indicator-management {
    @apply flex flex-col gap-4;

    .filter-bar {
        @apply bg-white rounded-lg p-4;
        box-shadow: $shadow-sm;
    }

    .filter-form {
        :deep(.el-form-item) {
            margin-bottom: 0;
        }

        :deep(.el-input) {
            width: 200px;
        }

        :deep(.el-select) {
            width: 180px;
        }
    }

    .table-card {
        @apply bg-white rounded-lg p-4;
        box-shadow: $shadow-sm;
    }

    .pagination-bar {
        @apply flex justify-end bg-white rounded-lg p-4;
        box-shadow: $shadow-sm;
    }
}
</style>
```

---

## 10. 路由与状态管理

### 10.1 路由配置

为每个页面生成路由配置：

```typescript
// src/router/modules/indicator.ts
import type { RouteConfig } from "vue-router";

const indicatorRoutes: RouteConfig[] = [
    {
        path: "/indicator",
        name: "Indicator",
        component: () => import("@/views/indicator/index.vue"),
        meta: {
            title: "指标管理",
            icon: "el-icon-data-line",
            permission: "indicator:view",
        },
    },
    {
        path: "/indicator/category",
        name: "IndicatorCategory",
        component: () => import("@/views/indicator/category.vue"),
        meta: {
            title: "指标分类管理",
            icon: "el-icon-collection",
            permission: "indicator:category:view",
        },
    },
];

export default indicatorRoutes;
```

### 10.2 Vuex Store（按需）

仅在跨组件共享数据时使用 Vuex：

```typescript
// src/store/modules/indicator.ts
import type { IndicatorItem } from "@/api/indicator";

interface IndicatorState {
    currentIndicator: IndicatorItem | null;
    domainList: string[];
}

const state: IndicatorState = {
    currentIndicator: null,
    domainList: [],
};

const mutations = {
    SET_CURRENT_INDICATOR(state: IndicatorState, indicator: IndicatorItem) {
        state.currentIndicator = indicator;
    },
    SET_DOMAIN_LIST(state: IndicatorState, list: string[]) {
        state.domainList = list;
    },
};

export default {
    namespaced: true,
    state,
    mutations,
};
```

---

## 11. 转化质量检查清单

转化完成后必须逐项检查：

### 11.1 代码规范

- [ ] 使用 `<script setup lang="ts">` 语法
- [ ] 使用 `<style lang="scss" scoped>` 语法
- [ ] 所有 Props 有完整 interface 定义
- [ ] 所有 Emits 有类型定义
- [ ] 无 `any` 类型
- [ ] 无魔法数字（使用枚举或常量）
- [ ] 文件名 kebab-case
- [ ] 组件引用 PascalCase
- [ ] 使用 Tailwind 工具类（优先于自定义 CSS）

### 11.2 Element UI 使用

- [ ] 表格使用 `<el-table>` + `<el-table-column>`
- [ ] 表单使用 `<el-form>` + `<el-form-item>`
- [ ] 弹窗使用 `<el-dialog>`
- [ ] 抽屉使用 `<el-drawer>`
- [ ] 输入框使用 `<el-input>`
- [ ] 下拉框使用 `<el-select>` + `<el-option>`
- [ ] 按钮使用 `<el-button>`
- [ ] 分页使用 `<el-pagination>`
- [ ] 删除有二次确认弹窗
- [ ] 消息提示使用 `ElMessage`

### 11.3 交互功能

- [ ] 查询/重置功能正常
- [ ] 分页切换正常
- [ ] 新增/编辑弹窗能打开和关闭
- [ ] 删除有确认弹窗
- [ ] 表格 loading 状态正常
- [ ] 空数据有 `<el-empty>` 提示
- [ ] 表单校验提示完整
- [ ] Tab 切换数据联动
- [ ] 图表随筛选变化

### 11.4 视觉还原

- [ ] 配色与原版一致
- [ ] 圆角大小一致
- [ ] 间距关系一致
- [ ] 字体大小层级一致
- [ ] 表格列宽合理
- [ ] 无横向溢出
- [ ] 无元素重叠

### 11.5 Mock 数据

- [ ] Mock 数据至少 20 条
- [ ] 分页逻辑正确
- [ ] 筛选逻辑正确
- [ ] USE_MOCK 切换标记明确
- [ ] API 接口定义完整

---

## 12. 注意事项

1. **不做过度抽象**：不要为了"通用"而创建过于复杂的组件封装，保持代码可读性优先
2. **保留业务语义**：原版 HTML 中的业务字段名、枚举值、交互逻辑必须完整保留
3. **一次性输出完整文件**：每次转化必须输出所有需要的文件，不要让研发自己去"补全"
4. **优先使用 Element UI 内置功能**：Element UI 已有的功能不要自己重新实现（如表单校验、表格排序、分页）
5. **大屏页面特殊处理**：大屏页面的布局通常不是 Element UI 标准布局，需要保留原版的 flex/grid 布局方式，但组件内部使用 Element UI
6. **接口路径约定**：API 路径使用 `/api/{module}/{action}` 格式，研发后端按此规范对接
