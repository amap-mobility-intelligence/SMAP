<script lang="ts" setup>

// 实验结果图片数据
const experimentResults = [
  {
    name: 'Table 1',
    label: 'Main Results',
    title: 'Table 1. Main Results of Different Methods on Our Dataset',
    image: './icon/Rusult_1.png',
  },
  {
    name: 'Table 2',
    label: 'Ablation: Multimodal',
    title: 'Table 2. Ablation Study on Multimodal Input',
    image: './icon/Result_2.png',
  },
  {
    name: 'Table 3',
    label: 'Ablation: HDPO',
    title: 'Table 3. Ablation Study on HDPO Sample Construction',
    image: './icon/Result_3.png',
  },
]
</script>

<template>
    <div>
        <el-divider />

        <el-row justify="center">
            <h1 class="section-title">Experiments</h1>
        </el-row>
        
        <!-- 实验结果展示 -->
        <el-row justify="center">
            <el-col :xs="24" :sm="22" :md="20" :lg="18" :xl="18">

                <!-- 卡片 -->
                <el-card class="card">

                    <!-- Tab 切换 -->
                    <el-tabs class="demo-tabs" model-value="Table 1">

                        <el-tab-pane 
                            v-for="exp in experimentResults" 
                            :key="exp.name"
                            :label="exp.label" 
                            :name="exp.name"
                        >
                            <!-- 表格标题 -->
                            <h3 class="table-title">{{ exp.title }}</h3>
                            
                            <!-- 结果图片 -->
                            <el-image 
                                :src="exp.image" 
                                fit="contain"
                                class="result-image"
                            />
                        </el-tab-pane>

                    </el-tabs>

                </el-card>
            </el-col>
        </el-row>

    </div>
</template>

<style scoped>
.card {
    margin-top: 20px;
}

.table-title {
    text-align: center;
    font-size: 16px;
    font-weight: bold;
    margin-bottom: 16px;
    color: #333;
}

.result-image {
    width: 100%;
    border-radius: 4px;
}
</style>
  