<script setup>
import { ref, onMounted, computed } from 'vue';

// 示例数据
const studyCases = ref([
  {
    id: 'preference_pair',
    title: 'HDPO Preference Pair Example',
    description: 'An example of chosen vs rejected response pair for HDPO training',
    query: '',
    responses: [
      {
        name: 'Rejected',
        mapImage: './study_case/preference_pair_example/reject.jpg',
        think: '',
        answer: ''
      },
      {
        name: 'Chosen',
        mapImage: './study_case/preference_pair_example/chosen.jpg',
        think: '',
        answer: ''
      }
    ]
  },
  {
    id: 'model_comparison',
    title: 'Model Comparison: SFT vs HDPO',
    description: 'Comparison between SFT baseline and HDPO model outputs',
    query: '',
    responses: [
      {
        name: 'SFT',
        mapImage: './study_case/response_example/sft.jpg',
        think: '',
        answer: ''
      },
      {
        name: 'HDPO',
        mapImage: './study_case/response_example/dpo.jpg',
        think: '',
        answer: ''
      }
    ]
  }
]);

const currentCaseIndex = ref(0);
const isLoading = ref(false);

// 加载数据
const loadStudyCases = async () => {
  isLoading.value = true;
  try {
    // 加载偏好对示例
    const prefReject = await fetch('./study_case/preference_pair_example/reject.json').then(r => r.json());
    const prefChosen = await fetch('./study_case/preference_pair_example/chosen.json').then(r => r.json());
    
    studyCases.value[0].query = 'Qinzhou retro film shooting route';
    studyCases.value[0].responses[0].think = prefReject.think;
    studyCases.value[0].responses[0].answer = prefReject.answer;
    studyCases.value[0].responses[1].think = prefChosen.think;
    studyCases.value[0].responses[1].answer = prefChosen.answer;
    
    // 加载模型对比示例
    const respSFT = await fetch('./study_case/response_example/sft.json').then(r => r.json());
    const respHDPO = await fetch('./study_case/response_example/dpo.json').then(r => r.json());
    
    studyCases.value[1].query = 'Must-visit itinerary in Macau';
    studyCases.value[1].responses[0].think = respSFT.think;
    studyCases.value[1].responses[0].answer = respSFT.answer;
    studyCases.value[1].responses[1].think = respHDPO.think;
    studyCases.value[1].responses[1].answer = respHDPO.answer;
    
    console.log('Study cases loaded successfully:', studyCases.value);
  } catch (error) {
    console.error('Failed to load study cases:', error);
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  loadStudyCases();
});

// 当前示例
const currentCase = computed(() => {
  if (studyCases.value.length === 0) return null;
  return studyCases.value[currentCaseIndex.value];
});

// 处理示例切换
const handleCaseChange = (value) => {
  currentCaseIndex.value = value;
};

// 格式化 think 内容，移除 Markdown 符号但保留结构
const formatThink = (text) => {
  if (!text) return '';
  return text
    // 移除加粗 **text**
    .replace(/\*\*(.*?)\*\*/g, '$1')
    // 移除斜体 *text* 或 _text_
    .replace(/\*(.*?)\*/g, '$1')
    .replace(/_(.*?)_/g, '$1')
    // 移除代码块 ```...```
    .replace(/```[\s\S]*?```/g, '')
    // 移除行内代码 `text`
    .replace(/`(.*?)`/g, '$1')
    // 将标题 ### text 转为 text
    .replace(/^#{1,6}\s+/gm, '')
    // 移除链接 [text](url) 保留 text
    .replace(/\[(.*?)\]\(.*?\)/g, '$1')
    // 移除图片
    .replace(/!\[.*?\]\(.*?\)/g, '')
    // 移除水平线
    .replace(/^-{3,}$/gm, '')
    // 清理多余的空行
    .replace(/\n{3,}/g, '\n\n')
    .trim();
};
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Study Cases</h1>
    </el-row>

    <!-- 示例切换 -->
    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="16" :lg="12" :xl="12">
        <!-- 自定义选项卡 -->
        <div class="custom-tabs">
          <div 
            v-for="(caseItem, index) in studyCases" 
            :key="caseItem.id"
            class="custom-tab"
            :class="{ active: currentCaseIndex === index }"
            @click="handleCaseChange(index)"
          >
            {{ caseItem.title }}
          </div>
        </div>

        <div class="case-content">
          <el-skeleton :rows="5" animated v-if="isLoading" />
          <template v-else-if="currentCase">
            <!-- Query 部分 -->
            <div class="query-section">
              <h3 class="section-label">Query:</h3>
              <div class="query-box">{{ currentCase.query }}</div>
            </div>

            <!-- 对比布局 -->
            <div class="comparison-wrapper">
              <!-- 第一行：SFT / Rejected -->
              <div class="comparison-item">
                <div class="row-label">{{ currentCase.id === 'preference_pair' ? 'Rejected' : 'SFT' }}</div>
                <el-row :gutter="20">
                  <el-col :span="12">
                    <div class="text-content">
                      <div class="think-display">
                        <span class="mini-label">Thinking Process:</span>
                        <div class="think-box">{{ formatThink(currentCase.responses[0].think) }}</div>
                      </div>
                      <div class="answer-display">
                        <span class="mini-label">Final Answer:</span>
                        <div class="answer-box">{{ currentCase.responses[0].answer }}</div>
                      </div>
                    </div>
                  </el-col>
                  <el-col :span="12">
                    <div class="image-content">
                      <div class="image-label">Generated Route:</div>
                      <img :src="currentCase.responses[0].mapImage" class="comparison-image" />
                    </div>
                  </el-col>
                </el-row>
              </div>

              <!-- 分隔线 -->
              <div class="divider-line"></div>

              <!-- 第二行：HDPO / Chosen -->
              <div class="comparison-item">
                <div class="row-label chosen">{{ currentCase.id === 'preference_pair' ? 'Chosen' : 'HDPO' }}</div>
                <el-row :gutter="20">
                  <el-col :span="12">
                    <div class="text-content">
                      <div class="think-display">
                        <span class="mini-label">Thinking Process:</span>
                        <div class="think-box">{{ formatThink(currentCase.responses[1].think) }}</div>
                      </div>
                      <div class="answer-display">
                        <span class="mini-label">Final Answer:</span>
                        <div class="answer-box">{{ currentCase.responses[1].answer }}</div>
                      </div>
                    </div>
                  </el-col>
                  <el-col :span="12">
                    <div class="image-content">
                      <div class="image-label">Generated Route:</div>
                      <img :src="currentCase.responses[1].mapImage" class="comparison-image" />
                    </div>
                  </el-col>
                </el-row>
              </div>
            </div>
          </template>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
.case-content {
  padding: 20px 0;
}

/* 自定义选项卡 */
.custom-tabs {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-top: 24px;
  padding: 8px;
  background: #f5f5f5;
  border-radius: 12px;
  max-width: fit-content;
  margin-left: auto;
  margin-right: auto;
}

.custom-tab {
  padding: 10px 24px;
  font-size: 14px;
  font-weight: 500;
  color: #666;
  background: transparent;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.custom-tab:hover {
  color: #333;
  background: rgba(255, 255, 255, 0.5);
}

.custom-tab.active {
  color: #fff;
  background: #409eff;
  box-shadow: 0 2px 8px rgba(64, 158, 255, 0.3);
}

.query-section {
  margin-bottom: 20px;
}

.section-label {
  font-size: 14px;
  font-weight: bold;
  color: #666;
  margin-bottom: 8px;
}

.query-box {
  font-size: 16px;
  line-height: 1.75rem;
  padding: 12px 16px;
  background: #fff;
  border-radius: 8px;
  border: 1px solid #e0e0e0;
}

.image-section {
  text-align: left;
  margin: 12px 0;
}

.study-case-image {
  max-width: 100%;
  max-height: 300px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.comparison-wrapper {
  margin-top: 12px;
  padding: 24px;
  background: #fff;
  border-radius: 12px;
  border: 1px solid #e0e0e0;
}

.comparison-item {
  padding: 0;
}

.comparison-item:first-child {
  padding-top: 0;
}

.comparison-item:last-child {
  padding-bottom: 0;
}

.divider-line {
  height: 2px;
  background: linear-gradient(to right, transparent, #d0d0d0, transparent);
  margin: 8px 0;
}

.row-label {
  font-size: 16px;
  font-weight: bold;
  color: #f56c6c;
  margin-bottom: 15px;
  padding: 6px 12px;
  background: #fef0f0;
  border-radius: 6px;
  display: inline-block;
}

.row-label.chosen {
  color: #67c23a;
  background: #f0f9eb;
}

.text-content {
  display: flex;
  flex-direction: column;
  gap: 12px;
  height: 100%;
  min-height: 280px;
}

.image-label {
  font-size: 12px;
  font-weight: bold;
  color: #666;
  margin-bottom: 4px;
}

.mini-label {
  font-size: 12px;
  font-weight: bold;
  color: #666;
  display: block;
  margin-bottom: 4px;
}

.mini-text {
  font-size: 13px;
  color: #333;
}



.think-box {
  background: #fff;
  border-radius: 6px;
  padding: 10px;
  font-size: 13px;
  line-height: 1.5;
  color: #333;
  white-space: pre-wrap;
  word-wrap: break-word;
  max-height: 220px;
  overflow-y: auto;
  border: 1px solid #e8e8e8;
}

.answer-box {
  background: #e8f4ff;
  border-radius: 6px;
  padding: 10px;
  font-size: 13px;
  color: #333;
  border-left: 3px solid #67c23a;
  white-space: pre-wrap;
  word-wrap: break-word;
}

.image-content {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  padding-top: 0;
  min-height: 280px;
}

.comparison-image {
  width: 100%;
  height: 100%;
  min-height: 250px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  object-fit: cover;
}



/* 滑块背景颜色 */
:deep(.el-slider__runway) {
  background-color: #c6c6c6;
}
</style>
