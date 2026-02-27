<script setup>
import { ref, onMounted, computed } from 'vue';

const datasetData = ref([]);
const currentIndex = ref(0);
const isLoading = ref(true);

// 加载数据集信息
const loadDataset = async () => {
  try {
    const response = await fetch('./data_demo/query_info.json');
    const data = await response.json();
    datasetData.value = data;
    isLoading.value = false;
  } catch (error) {
    console.error('Failed to load dataset:', error);
  }
};

onMounted(() => {
  loadDataset();
});

// 当前展示的数据
const currentData = computed(() => {
  if (datasetData.value.length === 0) return null;
  const originalData = datasetData.value[currentIndex.value];
  // 创建副本并修正图片路径
  const data = { ...originalData };
  if (data && data.image_path) {
    data.image_path = data.image_path.replace('demo/images', 'data_demo/images');
  }
  return data;
});

// 处理滑块变化
const handleChange = (value) => {
  currentIndex.value = value;
};

// 格式化POI列表，只显示前3个
const formatPOIList = (poiList) => {
  if (!poiList || poiList.length === 0) return [];
  const displayCount = Math.min(3, poiList.length);
  return poiList.slice(0, displayCount);
};

// 是否有更多POI
const hasMorePOI = (poiList) => {
  return poiList && poiList.length > 3;
};
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Dataset Demo</h1>
    </el-row>

    <!-- 数据展示区域 -->
    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="16" :lg="12" :xl="12">
        <el-card class="dataset-card" v-loading="isLoading">
          <template v-if="currentData">
            <!-- Query 部分 -->
            <div class="query-section">
              <h3 class="section-label">Query:</h3>
              <p class="query-text">{{ currentData.query }}</p>
            </div>

            <!-- 图片部分 -->
            <div class="image-section">
              <h3 class="section-label">Map Tile:</h3>
              <el-image 
                :src="currentData.image_path" 
                fit="contain"
                class="dataset-image"
              />
            </div>

            <!-- POI Info 部分 -->
            <div class="poi-section">
              <h3 class="section-label">Retrieved POIs:</h3>
              <div class="poi-list-scrollable">
                <div 
                  v-for="(poi, index) in currentData.poi_info_list" 
                  :key="index"
                  class="poi-item"
                >
                  <div class="poi-header">
                    <span class="poi-index">{{ index + 1 }}.</span>
                    <span class="poi-name">{{ poi.name }}</span>
                  </div>
                  <div class="poi-details">
                    <span class="poi-coor">📍 {{ poi.coor }}</span>
                    <span class="poi-duration" v-if="poi.duration">⏱ {{ poi.duration }}h</span>
                  </div>
                  <div class="poi-tags" v-if="poi.tag && poi.tag.length > 0">
                    <el-tag 
                      v-for="(tag, tagIndex) in poi.tag.slice(0, 3)" 
                      :key="tagIndex"
                      size="small"
                      class="poi-tag"
                    >
                      {{ tag }}
                    </el-tag>
                  </div>
                </div>
              </div>
            </div>

            <!-- 数据索引指示器 -->
            <div class="index-indicator">
              Sample {{ currentIndex + 1 }} / {{ datasetData.length }}
            </div>
          </template>
        </el-card>
      </el-col>
    </el-row>

    <!-- 滑块控制 -->
    <el-row justify="center" v-if="datasetData.length > 0">
      <el-col :xs="24" :sm="20" :md="16" :lg="12" :xl="12">
        <el-slider 
          v-model="currentIndex" 
          :min="0" 
          :max="datasetData.length - 1" 
          @input="handleChange"
          class="dataset-slider"
        />
      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
.dataset-card {
  margin: 20px 0;
  padding: 20px;
}

.query-section {
  margin-bottom: 12px;
}

.section-label {
  font-size: 14px;
  font-weight: bold;
  color: #666;
  margin-bottom: 8px;
}

.query-text {
  font-size: 16px;
  line-height: 1.75rem;
}

.image-section {
  text-align: left;
  margin: 12px 0;
}

.dataset-image {
  width: 100%;
  height: auto;
  max-height: 500px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  object-fit: contain;
}

.poi-section {
  margin-top: 20px;
}

.poi-list-scrollable {
  display: flex;
  flex-direction: column;
  gap: 12px;
  max-height: 300px;
  overflow-y: auto;
  padding-right: 8px;
}

.poi-item {
  padding: 12px;
  background: #f5f7fa;
  border-radius: 8px;
  border-left: 4px solid #409eff;
}

.poi-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 6px;
}

.poi-index {
  font-weight: bold;
  color: #409eff;
  font-size: 14px;
}

.poi-name {
  font-weight: bold;
  color: #333;
  font-size: 14px;
}

.poi-details {
  display: flex;
  gap: 16px;
  margin-bottom: 8px;
  font-size: 12px;
  color: #666;
}

.poi-coor, .poi-duration {
  display: flex;
  align-items: center;
  gap: 4px;
}

.poi-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.poi-tag {
  margin: 0;
}

/* 自定义滚动条样式 */
.poi-list-scrollable::-webkit-scrollbar {
  width: 6px;
}

.poi-list-scrollable::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 3px;
}

.poi-list-scrollable::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 3px;
}

.poi-list-scrollable::-webkit-scrollbar-thumb:hover {
  background: #a8a8a8;
}

.index-indicator {
  text-align: center;
  margin-top: 20px;
  padding-top: 15px;
  border-top: 1px solid #e0e0e0;
  color: #666;
  font-size: 14px;
}

.dataset-slider {
  margin: 20px 0;
}

/* 滑块背景颜色 */
:deep(.el-slider__runway) {
  background-color: #c6c6c6;
}
</style>
