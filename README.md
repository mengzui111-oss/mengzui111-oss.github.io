<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SCI-90心理健康测评系统</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #4a6fa5;
            --secondary: #6b8cbc;
            --light: #eef2f7;
            --dark: #2c3e50;
            --success: #2ecc71;
            --warning: #f39c12;
            --danger: #e74c3c;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f7fa;
            color: var(--dark);
            line-height: 1.6;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 30px 0;
            text-align: center;
            border-radius: 10px;
            margin-bottom: 30px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }
        
        header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }
        
        .dimensions-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        
        .dimension-card {
            background: white;
            border-radius: 10px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
            transition: transform 0.3s, box-shadow 0.3s;
            cursor: pointer;
            border-top: 4px solid var(--primary);
        }
        
        .dimension-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
        }
        
        .dimension-card i {
            font-size: 2.5rem;
            color: var(--primary);
            margin-bottom: 15px;
        }
        
        .dimension-card h3 {
            margin-bottom: 10px;
            color: var(--dark);
        }
        
        .dimension-card p {
            font-size: 0.9rem;
            color: #666;
        }
        
        .assessment-section {
            background: white;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            margin-bottom: 30px;
            display: none;
        }
        
        .assessment-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid #eee;
        }
        
        .assessment-header h2 {
            color: var(--primary);
        }
        
        .question {
            margin-bottom: 25px;
            padding: 15px;
            border-radius: 8px;
            background-color: var(--light);
        }
        
        .question-text {
            font-size: 1.1rem;
            margin-bottom: 15px;
            font-weight: 500;
        }
        
        .options {
            display: flex;
            justify-content: space-between;
        }
        
        .option {
            flex: 1;
            text-align: center;
            padding: 10px;
            margin: 0 5px;
            background: white;
            border: 1px solid #ddd;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .option:hover {
            background: #f0f0f0;
        }
        
        .option.selected {
            background: var(--primary);
            color: white;
            border-color: var(--primary);
        }
        
        .btn {
            padding: 12px 25px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 500;
            transition: all 0.3s;
        }
        
        .btn-primary {
            background: var(--primary);
            color: white;
        }
        
        .btn-primary:hover {
            background: #3a5a8a;
        }
        
        .btn-secondary {
            background: #e0e0e0;
            color: var(--dark);
        }
        
        .btn-secondary:hover {
            background: #d0d0d0;
        }
        
        .navigation {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
        }
        
        .result-section {
            background: white;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            margin-bottom: 30px;
            display: none;
        }
        
        .result-header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .result-header h2 {
            color: var(--primary);
            margin-bottom: 10px;
        }
        
        .result-summary {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
        }
        
        .chart-container {
            flex: 1;
            padding: 20px;
        }
        
        .score-summary {
            flex: 1;
            padding: 20px;
        }
        
        .dimension-results {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        
        .dimension-result {
            background: var(--light);
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid var(--primary);
        }
        
        .dimension-result h4 {
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
        }
        
        .score-bar {
            height: 10px;
            background: #e0e0e0;
            border-radius: 5px;
            margin-top: 5px;
            overflow: hidden;
        }
        
        .score-fill {
            height: 100%;
            border-radius: 5px;
            transition: width 0.5s;
        }
        
        .low {
            background: var(--success);
        }
        
        .medium {
            background: var(--warning);
        }
        
        .high {
            background: var(--danger);
        }
        
        .interpretation {
            margin-top: 10px;
            font-size: 0.9rem;
            color: #555;
        }
        
        .tab-container {
            display: flex;
            border-bottom: 1px solid #ddd;
            margin-bottom: 20px;
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
        }
        
        .tab.active {
            border-bottom: 3px solid var(--primary);
            color: var(--primary);
            font-weight: 500;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        footer {
            text-align: center;
            padding: 20px;
            color: #666;
            font-size: 0.9rem;
            margin-top: 30px;
        }
        
        @media (max-width: 768px) {
            .dimensions-grid {
                grid-template-columns: 1fr;
            }
            
            .result-summary {
                flex-direction: column;
            }
            
            .options {
                flex-direction: column;
            }
            
            .option {
                margin: 5px 0;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>SCI-90心理健康测评系统</h1>
            <p>全方位九大维度测评，科学评估您的心理健康状况</p>
        </header>
        
        <div class="dimensions-grid" id="dimensionsGrid">
            <!-- 维度卡片将通过JavaScript动态生成 -->
        </div>
        
        <div class="assessment-section" id="assessmentSection">
            <div class="assessment-header">
                <h2 id="currentDimension">维度名称</h2>
                <div>
                    <span id="currentQuestion">1</span>/<span id="totalQuestions">10</span>
                </div>
            </div>
            
            <div id="questionsContainer">
                <!-- 问题将通过JavaScript动态生成 -->
            </div>
            
            <div class="navigation">
                <button class="btn btn-secondary" id="prevBtn">上一题</button>
                <button class="btn btn-primary" id="nextBtn">下一题</button>
                <button class="btn btn-primary" id="submitBtn" style="display: none;">提交测评</button>
            </div>
        </div>
        
        <div class="result-section" id="resultSection">
            <div class="result-header">
                <h2>测评结果</h2>
                <p>以下是您在九个维度上的测评结果</p>
            </div>
            
            <div class="tab-container">
                <div class="tab active" data-tab="summary">总体结果</div>
                <div class="tab" data-tab="details">详细分析</div>
            </div>
            
            <div class="tab-content active" id="summaryTab">
                <div class="result-summary">
                    <div class="chart-container">
                        <canvas id="resultChart"></canvas>
                    </div>
                    <div class="score-summary">
                        <h3>总体得分: <span id="overallScore">0</span></h3>
                        <p id="overallInterpretation">总体解释将显示在这里</p>
                        <div class="dimension-results" id="dimensionResults">
                            <!-- 维度结果将通过JavaScript动态生成 -->
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="tab-content" id="detailsTab">
                <div id="detailedResults">
                    <!-- 详细结果将通过JavaScript动态生成 -->
                </div>
            </div>
            
            <div style="text-align: center; margin-top: 20px;">
                <button class="btn btn-primary" id="newAssessment">重新测评</button>
            </div>
        </div>
    </div>
    
    <footer>
        <p>SCI-90心理健康测评系统 &copy; 2023 | 本测评结果仅供参考，如有需要请咨询专业心理医生</p>
    </footer>

    <script>
        // 九大维度数据
        const dimensions = [
            {
                id: 'somatization',
                name: '躯体化',
                icon: 'fas fa-heartbeat',
                description: '反映主观的身体不适感',
                questions: [
                    '头痛',
                    '头晕或晕眩',
                    '胸痛',
                    '腰痛',
                    '恶心或胃部不适',
                    '肌肉酸痛',
                    '呼吸困难',
                    '一阵阵发冷或发热',
                    '身体发麻或刺痛',
                    '喉咙有梗塞感'
                ]
            },
            {
                id: 'obsessive_compulsive',
                name: '强迫症状',
                icon: 'fas fa-redo',
                description: '指那些明知没有必要，但又无法摆脱的无意义的思想、冲动和行为',
                questions: [
                    '忘记性大',
                    '担心自己的衣饰不整齐及仪态不端正',
                    '难以做出决定',
                    '感到难以完成任务',
                    '必须反复洗手、点数目或触摸某些东西',
                    '难以集中注意力',
                    '必须反复检查已经完成的事情',
                    '做事必须做得很慢以保证做得正确',
                    '头脑中有不必要的想法或字句盘旋',
                    '不能集中注意力'
                ]
            },
            {
                id: 'interpersonal_sensitivity',
                name: '人际关系敏感',
                icon: 'fas fa-users',
                description: '指某些个人不自在感与自卑感',
                questions: [
                    '对旁人责备求全',
                    '同异性相处时感到害羞不自在',
                    '感到别人不理解您、不同情您',
                    '感到人们对您不友好，不喜欢您',
                    '感到比不上他人',
                    '当别人看着您或谈论您时感到不自在',
                    '感到对别人神经过敏',
                    '感到在公共场合吃东西很不舒服',
                    '经常与人争论',
                    '感到孤独'
                ]
            },
            {
                id: 'depression',
                name: '抑郁',
                icon: 'fas fa-sad-tear',
                description: '反映与临床抑郁症状群相联系的广泛概念',
                questions: [
                    '对异性的兴趣减退',
                    '感到自己精力下降，活动减慢',
                    '想结束自己的生命',
                    '容易哭泣',
                    '感到受骗、中了圈套或有人想抓住您',
                    '感到孤独',
                    '感到苦闷',
                    '过分担忧',
                    '对事物不感兴趣',
                    '感到前途没有希望'
                ]
            },
            {
                id: 'anxiety',
                name: '焦虑',
                icon: 'fas fa-flushed',
                description: '指那些无法静息、神经过敏、紧张以及由此产生的躯体征象',
                questions: [
                    '神经过敏，心中不踏实',
                    '发抖',
                    '感到害怕',
                    '感到紧张或容易紧张',
                    '心跳得很厉害',
                    '感到一阵阵恐惧或惊恐',
                    '感到坐立不安心神不定',
                    '感到熟悉的东西变成陌生或不像是真的',
                    '感到要很快把事情做完',
                    '无缘无故地突然感到害怕'
                ]
            },
            {
                id: 'hostility',
                name: '敌对',
                icon: 'fas fa-angry',
                description: '主要从思维、情感及行为三个方面来反映敌对的表现',
                questions: [
                    '容易烦恼和激动',
                    '自己不能控制地大发脾气',
                    '有想打人或伤害他人的冲动',
                    '有想摔坏或破坏东西的冲动',
                    '经常与人争论',
                    '大叫或摔东西',
                    '感到对别人神经过敏',
                    '感到别人对您有恶意',
                    '感到大多数人都不可信任',
                    '有一些别人没有的想法或念头'
                ]
            },
            {
                id: 'phobic_anxiety',
                name: '恐怖',
                icon: 'fas fa-frown',
                description: '反映对孤独、公共场合和交通工具等的恐怖',
                questions: [
                    '害怕空旷的场所或街道',
                    '怕单独出门',
                    '怕乘电车、公共汽车、地铁或火车',
                    '因为感到害怕而避开某些东西、场合或活动',
                    '在商店或电影院等人多的地方感到不自在',
                    '单独一人时神经很紧张',
                    '害怕会在公共场合昏倒',
                    '害怕死亡',
                    '害怕某些动物',
                    '怕到高的地方'
                ]
            },
            {
                id: 'paranoid_ideation',
                name: '偏执',
                icon: 'fas fa-user-secret',
                description: '主要指思维方面，如投射性思维、敌对、猜疑、关系妄想等',
                questions: [
                    '感到大多数人都不可信任',
                    '感到有人在监视您、谈论您',
                    '有一些别人没有的想法或念头',
                    '别人对您的成绩没有做出恰当的评价',
                    '感到别人想占您的便宜',
                    '感到别人不理解您、不同情您',
                    '感到人们对您不友好，不喜欢您',
                    '您必须加倍小心，因为有人在算计您',
                    '您认为应该因为自己的过错而受到惩罚',
                    '感到自己的身体有严重问题'
                ]
            },
            {
                id: 'psychoticism',
                name: '精神病性',
                icon: 'fas fa-brain',
                description: '反映各式各样的急性症状和行为',
                questions: [
                    '感到别人能控制您的思想',
                    '感到自己的身体有严重问题',
                    '从未感到和其他人很亲近',
                    '感到有人在监视您、谈论您',
                    '有一些别人没有的想法或念头',
                    '认为应该因为自己的过错而受到惩罚',
                    '感到自己没有什么价值',
                    '感到熟悉的东西变成陌生或不像是真的',
                    '听到旁人听不到的声音',
                    '看到不存在的东西'
                ]
            }
        ];

        // 全局变量
        let currentDimensionIndex = 0;
        let currentQuestionIndex = 0;
        let answers = {};
        let results = {};

        // 初始化函数
        function init() {
            renderDimensions();
            setupEventListeners();
            initializeAnswers();
        }

        // 渲染维度卡片
        function renderDimensions() {
            const dimensionsGrid = document.getElementById('dimensionsGrid');
            dimensionsGrid.innerHTML = '';
            
            dimensions.forEach((dimension, index) => {
                const card = document.createElement('div');
                card.className = 'dimension-card';
                card.dataset.index = index;
                card.innerHTML = `
                    <i class="${dimension.icon}"></i>
                    <h3>${dimension.name}</h3>
                    <p>${dimension.description}</p>
                `;
                dimensionsGrid.appendChild(card);
            });
        }

        // 初始化答案对象
        function initializeAnswers() {
            dimensions.forEach(dimension => {
                answers[dimension.id] = new Array(dimension.questions.length).fill(0);
            });
        }

        // 设置事件监听器
        function setupEventListeners() {
            // 维度卡片点击事件
            document.querySelectorAll('.dimension-card').forEach(card => {
                card.addEventListener('click', function() {
                    const index = parseInt(this.dataset.index);
                    startAssessment(index);
                });
            });
            
            // 上一题按钮
            document.getElementById('prevBtn').addEventListener('click', goToPreviousQuestion);
            
            // 下一题按钮
            document.getElementById('nextBtn').addEventListener('click', goToNextQuestion);
            
            // 提交按钮
            document.getElementById('submitBtn').addEventListener('click', submitAssessment);
            
            // 重新测评按钮
            document.getElementById('newAssessment').addEventListener('click', resetAssessment);
            
            // 标签切换
            document.querySelectorAll('.tab').forEach(tab => {
                tab.addEventListener('click', function() {
                    const tabId = this.dataset.tab;
                    switchTab(tabId);
                });
            });
        }

        // 开始测评
        function startAssessment(dimensionIndex) {
            currentDimensionIndex = dimensionIndex;
            currentQuestionIndex = 0;
            
            const dimension = dimensions[dimensionIndex];
            document.getElementById('currentDimension').textContent = dimension.name;
            document.getElementById('totalQuestions').textContent = dimension.questions.length;
            
            renderQuestions();
            
            document.getElementById('dimensionsGrid').style.display = 'none';
            document.getElementById('assessmentSection').style.display = 'block';
            document.getElementById('resultSection').style.display = 'none';
            
            updateNavigationButtons();
        }

        // 渲染问题
        function renderQuestions() {
            const questionsContainer = document.getElementById('questionsContainer');
            questionsContainer.innerHTML = '';
            
            const dimension = dimensions[currentDimensionIndex];
            const question = dimension.questions[currentQuestionIndex];
            
            const questionElement = document.createElement('div');
            questionElement.className = 'question';
            questionElement.innerHTML = `
                <div class="question-text">${currentQuestionIndex + 1}. ${question}</div>
                <div class="options">
                    <div class="option" data-value="1">没有</div>
                    <div class="option" data-value="2">很轻</div>
                    <div class="option" data-value="3">中等</div>
                    <div class="option" data-value="4">偏重</div>
                    <div class="option" data-value="5">严重</div>
                </div>
            `;
            
            questionsContainer.appendChild(questionElement);
            
            // 设置选项点击事件
            document.querySelectorAll('.option').forEach(option => {
                option.addEventListener('click', function() {
                    // 移除其他选项的选中状态
                    this.parentElement.querySelectorAll('.option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    
                    // 设置当前选项为选中状态
                    this.classList.add('selected');
                    
                    // 保存答案
                    const dimensionId = dimensions[currentDimensionIndex].id;
                    answers[dimensionId][currentQuestionIndex] = parseInt(this.dataset.value);
                });
            });
            
            // 恢复之前的选择（如果有）
            const dimensionId = dimensions[currentDimensionIndex].id;
            const currentAnswer = answers[dimensionId][currentQuestionIndex];
            if (currentAnswer > 0) {
                const options = document.querySelectorAll('.option');
                options[currentAnswer - 1].classList.add('selected');
            }
            
            // 更新当前问题编号
            document.getElementById('currentQuestion').textContent = currentQuestionIndex + 1;
        }

        // 更新导航按钮状态
        function updateNavigationButtons() {
            const dimension = dimensions[currentDimensionIndex];
            const prevBtn = document.getElementById('prevBtn');
            const nextBtn = document.getElementById('nextBtn');
            const submitBtn = document.getElementById('submitBtn');
            
            // 上一题按钮
            if (currentQuestionIndex === 0) {
                prevBtn.style.display = 'none';
            } else {
                prevBtn.style.display = 'inline-block';
            }
            
            // 下一题/提交按钮
            if (currentQuestionIndex === dimension.questions.length - 1) {
                nextBtn.style.display = 'none';
                submitBtn.style.display = 'inline-block';
            } else {
                nextBtn.style.display = 'inline-block';
                submitBtn.style.display = 'none';
            }
        }

        // 上一题
        function goToPreviousQuestion() {
            if (currentQuestionIndex > 0) {
                currentQuestionIndex--;
                renderQuestions();
                updateNavigationButtons();
            }
        }

        // 下一题
        function goToNextQuestion() {
            const dimension = dimensions[currentDimensionIndex];
            if (currentQuestionIndex < dimension.questions.length - 1) {
                currentQuestionIndex++;
                renderQuestions();
                updateNavigationButtons();
            }
        }

        // 提交测评
        function submitAssessment() {
            calculateResults();
            displayResults();
            
            document.getElementById('assessmentSection').style.display = 'none';
            document.getElementById('resultSection').style.display = 'block';
        }

        // 计算结果
        function calculateResults() {
            results = {};
            
            dimensions.forEach(dimension => {
                const dimensionAnswers = answers[dimension.id];
                const sum = dimensionAnswers.reduce((a, b) => a + b, 0);
                const avg = sum / dimensionAnswers.length;
                
                results[dimension.id] = {
                    name: dimension.name,
                    score: avg,
                    interpretation: getInterpretation(avg)
                };
            });
            
            // 计算总体得分
            const allScores = Object.values(results).map(r => r.score);
            const overallScore = allScores.reduce((a, b) => a + b, 0) / allScores.length;
            results.overall = {
                score: overallScore,
                interpretation: getOverallInterpretation(overallScore)
            };
        }

        // 获取维度解释
        function getInterpretation(score) {
            if (score < 1.5) return { text: '正常范围', level: 'low' };
            if (score < 2.5) return { text: '轻度症状', level: 'medium' };
            if (score < 3.5) return { text: '中度症状', level: 'high' };
            return { text: '重度症状', level: 'high' };
        }

        // 获取总体解释
        function getOverallInterpretation(score) {
            if (score < 1.5) return '您的心理健康状况良好，各项指标均在正常范围内。';
            if (score < 2.5) return '您存在一些轻微的心理困扰，建议关注并适当调整。';
            if (score < 3.5) return '您存在中度心理困扰，建议寻求专业心理咨询。';
            return '您存在较为严重的心理困扰，强烈建议寻求专业心理医生的帮助。';
        }

        // 显示结果
        function displayResults() {
            // 更新总体得分
            document.getElementById('overallScore').textContent = results.overall.score.toFixed(2);
            document.getElementById('overallInterpretation').textContent = results.overall.interpretation;
            
            // 渲染维度结果
            const dimensionResults = document.getElementById('dimensionResults');
            dimensionResults.innerHTML = '';
            
            dimensions.forEach(dimension => {
                const result = results[dimension.id];
                const interpretation = result.interpretation;
                
                const resultElement = document.createElement('div');
                resultElement.className = 'dimension-result';
                resultElement.innerHTML = `
                    <h4>${dimension.name} <span>${result.score.toFixed(2)}</span></h4>
                    <div class="score-bar">
                        <div class="score-fill ${interpretation.level}" style="width: ${(result.score / 5) * 100}%"></div>
                    </div>
                    <div class="interpretation">${interpretation.text}</div>
                `;
                
                dimensionResults.appendChild(resultElement);
            });
            
            // 渲染详细结果
            const detailedResults = document.getElementById('detailedResults');
            detailedResults.innerHTML = '';
            
            dimensions.forEach(dimension => {
                const result = results[dimension.id];
                const interpretation = result.interpretation;
                
                const detailElement = document.createElement('div');
                detailElement.className = 'dimension-result';
                detailElement.innerHTML = `
                    <h4>${dimension.name} <span>${result.score.toFixed(2)}</span></h4>
                    <p>${dimension.description}</p>
                    <div class="score-bar">
                        <div class="score-fill ${interpretation.level}" style="width: ${(result.score / 5) * 100}%"></div>
                    </div>
                    <div class="interpretation">
                        <strong>解释:</strong> ${interpretation.text}
                    </div>
                `;
                
                detailedResults.appendChild(detailElement);
            });
            
            // 渲染图表
            renderChart();
        }

        // 渲染图表
        function renderChart() {
            const ctx = document.getElementById('resultChart').getContext('2d');
            
            const labels = dimensions.map(d => d.name);
            const data = dimensions.map(d => results[d.id].score);
            const backgroundColors = dimensions.map(d => {
                const level = results[d.id].interpretation.level;
                if (level === 'low') return 'rgba(46, 204, 113, 0.7)';
                if (level === 'medium') return 'rgba(243, 156, 18, 0.7)';
                return 'rgba(231, 76, 60, 0.7)';
            });
            
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: '得分',
                        data: data,
                        backgroundColor: backgroundColors,
                        borderColor: backgroundColors.map(c => c.replace('0.7', '1')),
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 5,
                            title: {
                                display: true,
                                text: '得分'
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        title: {
                            display: true,
                            text: '九大维度得分'
                        }
                    }
                }
            });
        }

        // 切换标签
        function switchTab(tabId) {
            // 更新标签激活状态
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            document.querySelector(`.tab[data-tab="${tabId}"]`).classList.add('active');
            
            // 更新内容显示
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.remove('active');
            });
            document.getElementById(`${tabId}Tab`).classList.add('active');
        }

        // 重置测评
        function resetAssessment() {
            initializeAnswers();
            document.getElementById('dimensionsGrid').style.display = 'grid';
            document.getElementById('assessmentSection').style.display = 'none';
            document.getElementById('resultSection').style.display = 'none';
        }

        // 初始化应用
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html># mengzui111-oss.github.io
