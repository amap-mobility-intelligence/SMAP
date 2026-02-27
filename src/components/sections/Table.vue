<script lang="ts" setup>

// 表格列定义
const tableColumns = [
  { prop: 'model', label: 'Model', width: '350', fixed: 'left' },
  { prop: 'psr', label: 'PSR↑', width: '100', align: 'center' },
  { prop: 'rdr', label: 'RDR↑', width: '100', align: 'center' },
  { prop: 'rtpr', label: 'RTPR↑', width: '100', align: 'center' },
  { prop: 'tspr', label: 'TSPR↑', width: '100', align: 'center' },
  { prop: 'shr', label: 'SHR↓', width: '100', align: 'center' },
  { prop: 'ors', label: 'ORS↑', width: '100', align: 'center' },
  { prop: 'cpr', label: 'CPR↑', width: '100', align: 'center' },
]

// Table 1: Main Results
const table1Data = [
  // Pretrained Models
  { model: 'Qwen2.5-VL-7B', psr: '94.0', rdr: '0.632', rtpr: '53.7', tspr: '29.2', shr: '37.7', ors: '1.62', cpr: '0.5', group: 'Pretrained Models' },
  { model: 'Qwen2.5-VL-32B', psr: '97.5', rdr: '0.667', rtpr: '59.3', tspr: '34.5', shr: '31.9', ors: '1.69', cpr: '4.6', group: 'Pretrained Models' },
  { model: 'Qwen2.5-VL-72B', psr: '97.5', rdr: '0.662', rtpr: '71.7', tspr: '37.6', shr: '30.6', ors: '1.87', cpr: '6.7', group: 'Pretrained Models' },
  { model: 'GPT-4o', psr: '95.0', rdr: '0.688', rtpr: '72.8', tspr: '36.5', shr: '31.8', ors: '2.12', cpr: '10.5', group: 'Pretrained Models' },
  { model: 'OpenAI-o1', psr: '100', rdr: '0.780', rtpr: '90.9', tspr: '37.0', shr: '21.2', ors: '3.10', cpr: '15.0', group: 'Pretrained Models', bold: ['psr'] },
  { model: 'GPT-5', psr: '100', rdr: '0.825', rtpr: '94.0', tspr: '68.8', shr: '13.3', ors: '3.76', cpr: '-', group: 'Pretrained Models', bold: ['psr', 'rdr', 'rtpr', 'tspr', 'shr', 'ors'], highlight: true },
  // Post-trained Models
  { model: 'Qwen2.5-VL-7B-SFT', psr: '99.5', rdr: '0.763', rtpr: '83.8', tspr: '63.1', shr: '22.1', ors: '3.00', cpr: '29.6', group: 'Post-trained Models' },
  { model: 'Qwen2.5-VL-7B-HDPO', psr: '100', rdr: '0.802', rtpr: '85.4', tspr: '70.4', shr: '20.4', ors: '3.37', cpr: '34.0', group: 'Post-trained Models', bold: ['psr'] },
  { model: 'Qwen2.5-VL-32B-SFT', psr: '100', rdr: '0.789', rtpr: '91.0', tspr: '73.0', shr: '15.8', ors: '3.74', cpr: '48.0', group: 'Post-trained Models', bold: ['psr', 'rtpr'] },
  { model: 'Qwen2.5-VL-32B-HDPO', psr: '100', rdr: '0.831', rtpr: '91.0', tspr: '77.5', shr: '14.0', ors: '3.89', cpr: '51.5', group: 'Post-trained Models', bold: ['psr', 'rdr', 'rtpr', 'tspr', 'shr', 'ors', 'cpr'], highlight: true },
]

// Table 2: Ablation Study on Multimodal Input
const table2Data = [
  // Qwen2.5-VL-7B
  { model: 'SFT (text-only)', psr: '99.5', rdr: '0.726', rtpr: '84.8', tspr: '60.3', shr: '25.4', ors: '3.10', cpr: '29.6', group: 'Qwen2.5-VL-7B' },
  { model: 'HDPO (text-only)', psr: '99.5', rdr: '0.734', rtpr: '86.9', tspr: '68.2', shr: '22.7', ors: '3.39', cpr: '32.7', group: 'Qwen2.5-VL-7B', bold: ['rtpr', 'ors'] },
  { model: 'SFT (text-image)', psr: '99.5', rdr: '0.763', rtpr: '83.8', tspr: '63.1', shr: '22.1', ors: '3.00', cpr: '29.6', group: 'Qwen2.5-VL-7B' },
  { model: 'HDPO (text-image)', psr: '100', rdr: '0.802', rtpr: '85.4', tspr: '70.4', shr: '20.4', ors: '3.37', cpr: '34.0', group: 'Qwen2.5-VL-7B', bold: ['psr', 'rdr', 'tspr', 'shr', 'cpr'], highlight: true },
  // Qwen2.5-VL-32B
  { model: 'SFT (text-only)', psr: '100', rdr: '0.776', rtpr: '89.4', tspr: '70.5', shr: '19.2', ors: '3.53', cpr: '46.5', group: 'Qwen2.5-VL-32B', bold: ['psr'] },
  { model: 'HDPO (text-only)', psr: '100', rdr: '0.789', rtpr: '91.4', tspr: '73.5', shr: '15.4', ors: '3.72', cpr: '49.0', group: 'Qwen2.5-VL-32B', bold: ['psr', 'rtpr'] },
  { model: 'SFT (text-image)', psr: '100', rdr: '0.789', rtpr: '91.0', tspr: '73.0', shr: '15.8', ors: '3.74', cpr: '48.0', group: 'Qwen2.5-VL-32B', bold: ['psr'] },
  { model: 'HDPO (text-image)', psr: '100', rdr: '0.831', rtpr: '91.0', tspr: '77.5', shr: '14.0', ors: '3.89', cpr: '51.5', group: 'Qwen2.5-VL-32B', bold: ['psr', 'rdr', 'tspr', 'shr', 'ors', 'cpr'], highlight: true },
]

// Table 3: Ablation Study on HDPO Sample Construction
const table3Data = [
  // Qwen2.5-VL-7B
  { model: 'SFT', psr: '99.5', rdr: '0.763', rtpr: '83.8', tspr: '63.1', shr: '22.1', ors: '3.00', cpr: '29.6', group: 'Qwen2.5-VL-7B' },
  { model: 'DPO', psr: '100', rdr: '0.733', rtpr: '79.8', tspr: '58.0', shr: '27.6', ors: '2.75', cpr: '30.5', group: 'Qwen2.5-VL-7B', bold: ['psr'] },
  { model: 'HDPO', psr: '100', rdr: '0.802', rtpr: '85.4', tspr: '70.4', shr: '20.4', ors: '3.37', cpr: '34.0', group: 'Qwen2.5-VL-7B', bold: ['psr', 'rdr', 'rtpr', 'tspr', 'shr', 'ors', 'cpr'], highlight: true },
  // Qwen2.5-VL-32B
  { model: 'SFT', psr: '100', rdr: '0.789', rtpr: '91.0', tspr: '73.0', shr: '15.8', ors: '3.74', cpr: '48.0', group: 'Qwen2.5-VL-32B', bold: ['psr', 'rtpr'] },
  { model: 'DPO', psr: '100', rdr: '0.765', rtpr: '90.5', tspr: '72.0', shr: '16.2', ors: '3.76', cpr: '48.0', group: 'Qwen2.5-VL-32B', bold: ['psr'] },
  { model: 'HDPO', psr: '100', rdr: '0.831', rtpr: '91.0', tspr: '77.5', shr: '14.0', ors: '3.89', cpr: '51.5', group: 'Qwen2.5-VL-32B', bold: ['psr', 'rdr', 'rtpr', 'tspr', 'shr', 'ors', 'cpr'], highlight: true },
]

// 实验结果配置
const experimentResults = [
  {
    name: 'Table 1',
    label: 'Main Results',
    title: 'Table 1. Main Results of Different Methods on Our Dataset',
    data: table1Data,
  },
  {
    name: 'Table 2',
    label: 'Ablation: Multimodal',
    title: 'Table 2. Ablation Study on Multimodal Input',
    data: table2Data,
  },
  {
    name: 'Table 3',
    label: 'Ablation: HDPO',
    title: 'Table 3. Ablation Study on HDPO Sample Construction',
    data: table3Data,
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
            <el-col :xs="24" :sm="20" :md="16" :lg="12" :xl="12">

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
                            
                            <!-- 数据表格 -->
                            <div class="table-wrapper">
                                <table class="data-table">
                                    <thead>
                                        <tr>
                                            <th v-for="col in tableColumns" :key="col.prop" :style="{ width: col.width + 'px', textAlign: (col.align || 'left') as any }">
                                                {{ col.label }}
                                            </th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <template v-for="(row, index) in exp.data" :key="index">
                                            <!-- 分组标题行 -->
                                            <tr v-if="index === 0 || row.group !== exp.data[index - 1].group" class="group-row">
                                                <td :colspan="tableColumns.length" class="group-cell">
                                                    {{ row.group }}
                                                </td>
                                            </tr>
                                            <!-- 数据行 -->
                                            <tr :class="{ 'highlight-row': (row as any).highlight }">
                                                <td v-for="col in tableColumns" :key="col.prop" :style="{ textAlign: (col.align || 'left') as any }" :class="{ 'bold-cell': (row as any).bold && (row as any).bold.includes(col.prop) }">
                                                    {{ row[col.prop as keyof typeof row] }}
                                                </td>
                                            </tr>
                                        </template>
                                    </tbody>
                                </table>
                            </div>
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

/* 表格样式 */
.table-wrapper {
    overflow-x: auto;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
}

.data-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
}

.data-table thead {
    background-color: #f5f7fa;
}

.data-table th {
    padding: 12px 8px;
    font-weight: bold;
    color: #606266;
    border-bottom: 1px solid #e0e0e0;
    white-space: nowrap;
}

.data-table td {
    padding: 10px 8px;
    border-bottom: 1px solid #ebeef5;
}

.data-table tbody tr:hover {
    background-color: #f5f7fa;
}

/* 分组标题行样式 */
.group-row {
    background-color: #fafafa !important;
}

.group-cell {
    text-align: center !important;
    font-weight: bold;
    color: #909399;
    font-size: 12px;
    padding: 8px !important;
    background-color: #fafafa;
    border-bottom: 1px solid #e0e0e0;
}

/* 加粗单元格样式 */
.bold-cell {
    font-weight: bold;
}

/* 高亮行样式（蓝色背景） */
.highlight-row {
    background-color: #e6f7ff !important;
}

.highlight-row td {
    background-color: #e6f7ff !important;
}
</style>
  