<template>
    <div class="resume-upload-page">
      <!-- 左侧信息区域 -->
      <div class="left-panel">
        <!-- 文本内容区域 -->
        <div class="text-content">
          <h1 class="main-title">{{ stepTitles[currentStep - 1] }}</h1>
          <h2 class="sub-title">{{ stepSubTitles[currentStep - 1] }}</h2>
          <p class="description">
            {{ stepDescriptions[currentStep - 1] }}
          </p>
          <div class="step-marker">{{ currentStep }}</div>
        </div>
        
        <!-- 左下角图片区域 -->
        <div class="illustration">
          <img 
            src="/image.png" 
            alt="Resume Illustration"
            class="custom-image"
          />
        </div>
      </div>
  
      <!-- 右侧上传操作区域 -->
      <div class="right-panel">
        <div class="upload-card">
          <h2 class="panel-title">{{ stepPanelTitles[currentStep - 1] }}</h2>
          
          <!-- 上传区域 - 只在第三步显示 -->
          <div 
            v-if="currentStep === 3"
            class="upload-area"
            :class="{ 'drag-over': isDragOver, 'has-file': selectedFile }"
            @click="triggerFileInput"
            @drop="handleDrop"
            @dragover="handleDragOver"
            @dragleave="handleDragLeave"
          >
            <div class="upload-icon">
              <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="#4285f4" stroke-width="2">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/>
                <polyline points="17 8 12 3 7 8"/>
                <line x1="12" y1="3" x2="12" y2="15"/>
              </svg>
            </div>
            <p class="upload-text" v-if="!selectedFile">
              Drag your resume file to this area, or click on the area to select the appropriate file to upload
            </p>
            <div class="file-info" v-else>
              <div class="file-icon">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#4285f4" stroke-width="2">
                  <path d="M13 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"/>
                  <polyline points="13 2 13 9 20 9"/>
                </svg>
              </div>
              <div class="file-details">
                <p class="file-name">{{ selectedFile.name }}</p>
                <p class="file-size">{{ formatFileSize(selectedFile.size) }}</p>
              </div>
              <button class="remove-file" @click.stop="removeFile">×</button>
            </div>
            <input
              ref="fileInput"
              type="file"
              @change="handleFileSelect"
              accept=".pdf,.doc,.docx"
              style="display: none"
            />
          </div>
  
          <!-- 步骤内容区域 -->
          <div class="step-content" v-else>
            <p>This is step {{ currentStep }} content. You can add specific content for each step here.</p>
          </div>
  
          <!-- 按钮区域 -->
          <div class="button-group">
            <button class="btn btn-secondary" @click="handleLastStep" :disabled="currentStep === 1">
              Last step
            </button>
            <button 
              class="btn btn-primary" 
              @click="handleNextStep"
            >
              {{ currentStep === 3 ? 'Finish' : 'Next step' }}
            </button>
          </div>
  
          <!-- 步骤指示器 - 放在按钮下方 -->
          <div class="step-indicator">
            <div class="step-progress-bar">
              <div 
                class="step-item" 
                :class="{ 
                  'completed': currentStep > 1, 
                  'active': currentStep === 1 
                }"
              >
                <div class="step-icon">{{ currentStep > 1 ? '✓' : '1' }}</div>
              </div>
              <div class="step-line" :class="{ 'completed': currentStep > 1 }"></div>
              <div 
                class="step-item" 
                :class="{ 
                  'completed': currentStep > 2, 
                  'active': currentStep === 2 
                }"
              >
                <div class="step-icon">{{ currentStep > 2 ? '✓' : '2' }}</div>
              </div>
              <div class="step-line" :class="{ 'completed': currentStep > 2 }"></div>
              <div 
                class="step-item" 
                :class="{ 
                  'completed': currentStep > 3, 
                  'active': currentStep === 3 
                }"
              >
                <div class="step-icon">{{ currentStep > 3 ? '✓' : '3' }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue'
  
  // 步骤数据
  const currentStep = ref(3)
  
  // 步骤内容配置
  const stepTitles = ['Start!', 'Continue!', 'Finish!']
  const stepSubTitles = ['Begin Your Journey', 'Complete Your Profile', 'Upload Your Resume']
  const stepDescriptions = [
    'Start your application process by providing basic information',
    'Complete your profile details to help us understand you better',
    'Upload your resume, the platform will help you parse and optimize, you can also skip this step'
  ]
  const stepPanelTitles = ['Basic Information', 'Profile Details', 'Upload file']
  
  // 响应式数据
  const fileInput = ref(null)
  const selectedFile = ref(null)
  const isDragOver = ref(false)
  
  // 触发文件选择
  const triggerFileInput = () => {
    fileInput.value?.click()
  }
  
  // 处理文件选择
  const handleFileSelect = (event) => {
    const file = event.target.files[0]
    if (file) {
      selectedFile.value = file
    }
  }
  
  // 移除文件
  const removeFile = () => {
    selectedFile.value = null
    if (fileInput.value) {
      fileInput.value.value = ''
    }
  }
  
  // 格式化文件大小
  const formatFileSize = (bytes) => {
    if (bytes === 0) return '0 Bytes'
    const k = 1024
    const sizes = ['Bytes', 'KB', 'MB', 'GB']
    const i = Math.floor(Math.log(bytes) / Math.log(k))
    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i]
  }
  
  // 拖拽相关处理
  const handleDragOver = (event) => {
    event.preventDefault()
    isDragOver.value = true
  }
  
  const handleDragLeave = (event) => {
    event.preventDefault()
    isDragOver.value = false
  }
  
  const handleDrop = (event) => {
    event.preventDefault()
    isDragOver.value = false
    
    const files = event.dataTransfer.files
    if (files.length > 0) {
      selectedFile.value = files[0]
    }
  }
  
  // 上一步按钮处理
  const handleLastStep = () => {
    if (currentStep.value > 1) {
      currentStep.value--
    }
  }
  
  // 下一步/完成按钮处理
  const handleNextStep = () => {
    if (currentStep.value < 3) {
      currentStep.value++
    } else {
      // 完成操作
      if (selectedFile.value) {
        alert('Resume uploaded successfully!')
        // 这里可以添加实际的上传逻辑
        selectedFile.value = null
        if (fileInput.value) {
          fileInput.value.value = ''
        }
      } else {
        alert('Please select a file first.')
      }
    }
  }
  </script>
  
  <style scoped>
  .resume-upload-page {
    display: flex;
    min-height: 100vh;
    background-color: #f5f7fa;
    font-family: Arial, sans-serif;
    overflow: hidden;
  }
  
  /* 左侧信息区域样式 */
  .left-panel {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 2rem;
    position: relative;
  }
  
  /* 文本内容区域 */
  .text-content {
    max-width: 400px;
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
    margin-top: 2rem;
    margin-left: 2rem;
  }
  
  .main-title {
    font-size: 2rem;
    font-weight: 700;
    color: #1a1a1a;
    margin: 0;
  }
  
  .sub-title {
    font-size: 1.5rem;
    font-weight: 600;
    color: #4285f4;
    margin: 0;
  }
  
  .description {
    font-size: 0.875rem;
    color: #666666;
    line-height: 1.5;
    margin: 0;
  }
  
  .step-marker {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    background-color: #4285f4;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 600;
  }
  
  /* 左下角图片区域 */
  .illustration {
    width: 100%;
    max-width: 763px;
    height: auto;
    margin-top: auto;
    margin-left: 2rem;
    margin-bottom: 2rem;
  }
  
  .custom-image {
    width: 100%;
    height: auto;
    object-fit: contain;
  }
  
  /* 右侧上传区域样式 */
  .right-panel {
    flex: 0 0 600px;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
  }
  
  .upload-card {
    width: 100%;
    height: 700px;
    background: #ffffff;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    border-radius: 32px;
    padding: 2.5rem;
    display: flex;
    flex-direction: column;
  }
  
  .panel-title {
    font-size: 1.75rem;
    font-weight: 600;
    color: #1a1a1a;
    margin-bottom: 2rem;
    text-align: center;
  }
  
  /* 步骤内容区域 */
  .step-content {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    color: #666;
    font-size: 1.1rem;
    margin-bottom: 2rem;
  }
  
  /* 上传区域 */
  .upload-area {
    flex: 1;
    border: 2px dashed #4285f4;
    border-radius: 15px;
    padding: 2rem;
    text-align: center;
    cursor: pointer;
    transition: all 0.3s ease;
    background-color: #f8fcff;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-bottom: 2rem;
  }
  
  .upload-area.drag-over {
    background-color: #e8f3ff;
    border-color: #1B5AFF;
  }
  
  .upload-area.has-file {
    border-style: solid;
    background-color: #f0f8ff;
  }
  
  .upload-icon {
    margin-bottom: 1.5rem;
  }
  
  .upload-text {
    font-size: 1rem;
    color: #666666;
    margin: 0;
    line-height: 1.6;
    max-width: 400px;
  }
  
  /* 文件信息样式 */
  .file-info {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 1rem;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 400px;
  }
  
  .file-icon {
    flex-shrink: 0;
  }
  
  .file-details {
    flex: 1;
    text-align: left;
  }
  
  .file-name {
    font-weight: 600;
    color: #1a1a1a;
    margin: 0 0 0.25rem 0;
    font-size: 1rem;
  }
  
  .file-size {
    color: #666;
    margin: 0;
    font-size: 0.875rem;
  }
  
  .remove-file {
    background: none;
    border: none;
    font-size: 1.5rem;
    color: #999;
    cursor: pointer;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
  }
  
  .remove-file:hover {
    background: #f5f5f5;
    color: #ff4444;
  }
  
  /* 按钮区域 */
  .button-group {
    display: flex;
    gap: 1rem;
    justify-content: center;
    margin-bottom: 2rem;
  }
  
  .btn {
    width: 200px;
    height: 55px;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s ease;
    font-family: Arial, sans-serif;
  }
  
  .btn:active {
    transform: scale(0.98);
  }
  
  .btn-primary {
    background: #1B5AFF;
    color: white;
  }
  
  .btn-primary:hover:not(:disabled) {
    background: #3a78e0;
  }
  
  .btn-primary:disabled {
    background: #9ca3af;
    cursor: not-allowed;
    transform: none;
  }
  
  .btn-secondary {
    background: #75ACFF;
    color: white;
  }
  
  .btn-secondary:hover:not(:disabled) {
    background: #5aa5e0;
  }
  
  .btn-secondary:disabled {
    background: #c2c2c2;
    cursor: not-allowed;
    transform: none;
  }
  
  /* 步骤指示器 */
  .step-indicator {
    display: flex;
    justify-content: center;
    margin-top: auto;
  }
  
  .step-progress-bar {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .step-item {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.9rem;
    font-weight: 600;
    background: white;
    border: 2px solid #e0e0e0;
    transition: all 0.3s ease;
  }
  
  .step-item.completed {
    background: #85A7FF;
    border-color: #85A7FF;
    color: white;
  }
  
  .step-item.active {
    background: #4285f4;
    border-color: #4285f4;
    color: white;
  }
  
  .step-line {
    width: 60px;
    height: 3px;
    background: #e0e0e0;
    border-radius: 2px;
    transition: all 0.3s ease;
  }
  
  .step-line.completed {
    background: #85A7FF;
  }
  
  /* 响应式调整 */
  @media (max-width: 1440px) {
    .resume-upload-page {
      flex-direction: column;
      align-items: center;
      padding: 1rem;
    }
    
    .left-panel {
      width: 100%;
      max-width: 900px;
      padding: 1rem;
      text-align: center;
    }
    
    .text-content {
      max-width: 100%;
      margin: 1rem auto;
    }
    
    .illustration {
      width: 100%;
      max-width: 600px;
      height: auto;
      margin: 1rem auto;
    }
    
    .right-panel {
      flex: none;
      width: 100%;
      max-width: 600px;
      padding: 1rem;
    }
    
    .upload-card {
      height: auto;
      min-height: 600px;
    }
  }
  
  @media (max-width: 1024px) {
    .resume-upload-page {
      flex-direction: column;
    }
    
    .left-panel, .right-panel {
      width: 100%;
      padding: 1rem;
    }
    
    .right-panel {
      flex: none;
    }
    
    .upload-card {
      height: auto;
      min-height: 500px;
    }
    
    .button-group {
      flex-direction: column;
      align-items: center;
    }
    
    .btn {
      width: 100%;
      max-width: 272px;
    }
  }
  
  @media (max-width: 768px) {
    .left-panel {
      padding: 1rem;
    }
    
    .text-content {
      margin: 1rem 0;
    }
    
    .main-title {
      font-size: 1.75rem;
    }
    
    .sub-title {
      font-size: 1.25rem;
    }
    
    .right-panel {
      padding: 0.5rem;
    }
    
    .upload-card {
      padding: 2rem;
      border-radius: 24px;
      height: auto;
      min-height: 450px;
    }
    
    .upload-area {
      padding: 1.5rem;
    }
    
    .step-progress-bar {
      gap: 0.25rem;
    }
    
    .step-item {
      width: 28px;
      height: 28px;
      font-size: 0.8rem;
    }
    
    .step-line {
      width: 40px;
    }
  }
  
  @media (max-width: 480px) {
    .left-panel {
      padding: 0.5rem;
    }
    
    .text-content {
      gap: 1rem;
    }
    
    .main-title {
      font-size: 1.5rem;
    }
    
    .sub-title {
      font-size: 1.1rem;
    }
    
    .upload-card {
      padding: 1.5rem;
      border-radius: 20px;
    }
    
    .panel-title {
      font-size: 1.5rem;
      margin: 1rem 0;
    }
    
    .upload-area {
      padding: 1rem;
    }
    
    .upload-text {
      font-size: 0.9rem;
    }
    
    .file-info {
      flex-direction: column;
      text-align: center;
      gap: 0.5rem;
    }
    
    .file-details {
      text-align: center;
    }
    
    .step-line {
      width: 30px;
    }
    
    .btn {
      height: 50px;
      font-size: 0.9rem;
    }
  }
  </style>