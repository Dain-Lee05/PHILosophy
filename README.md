# PHILosophy
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Workplace Personality Quiz</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Nanum+Square+Round:wght@400;700;800&family=Quicksand:wght@500;700;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-color: #f0f4f8;
            --card-color: #ffffff;
            --primary-color: #1e3a8a;
            --accent-color: #3b82f6;
            --light-blue: #eff6ff;
            --text-dark: #1e293b;
            --text-light: #64748b;
            --border-color: #cbd5e1;
            --hover-color: #dbeafe;
            --success-color: #10b981;
            --danger-color: #ef4444;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Nanum Square Round', 'Quicksand', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-dark);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 10px;
        }

        /* Strict iPhone 9:16 Frame */
        .quiz-container {
            width: 100%;
            max-width: 375px;
            height: 667px;
            background: var(--card-color);
            border-radius: 32px;
            box-shadow: 0 15px 35px rgba(30, 58, 138, 0.12);
            padding: 20px;
            position: relative;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        /* Enforced Display States to Prevent White Screens */
        .page {
            display: none;
            width: 100%;
            height: 100%;
            flex-direction: column;
            justify-content: space-between;
            opacity: 0;
            transform: translateY(10px);
            transition: transform 0.25s ease, opacity 0.25s ease;
        }

        .page.active {
            display: flex !important;
            opacity: 1;
            transform: translateY(0);
        }

        .page.exit {
            transform: translateY(-10px);
            opacity: 0;
        }

        .top-content {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
        }

        h1 {
            font-size: 18px;
            font-weight: 800;
            text-align: center;
            color: var(--primary-color);
            margin-top: 10px;
            margin-bottom: 6px;
            line-height: 1.3;
        }

        /* Moved Up & Centered Title spacing */
        .philosophy-title {
            font-size: 32px;
            font-weight: 500;
            text-align: center;
            color: var(--text-light);
            letter-spacing: 1px;
            margin-top: 15px;
            margin-bottom: 15px;
            font-family: 'Quicksand', sans-serif;
        }

        .philosophy-title span {
            font-weight: 800;
            color: var(--accent-color);
        }

        .philosophy-box {
            background-color: var(--light-blue);
            border-left: 4px solid var(--accent-color);
            padding: 16px;
            border-radius: 12px;
            font-size: 13.5px;
            line-height: 1.6;
            color: #334155;
            text-align: left;
        }

        .subtitle {
            font-size: 12px;
            color: var(--text-light);
            text-align: center;
            margin-bottom: 10px;
        }

        /* Highly Compact Choices Container to Avoid Layout Clipping */
        .btn-grid {
            display: flex;
            flex-direction: column;
            gap: 8px;
            width: 100%;
            overflow-y: auto;
            max-height: 380px;
            padding-right: 2px;
            margin-top: 5px;
            margin-bottom: 5px;
        }

        .btn-grid::-webkit-scrollbar {
            width: 4px;
        }
        .btn-grid::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 4px;
        }

        button {
            font-family: 'Nanum Square Round', sans-serif;
            font-size: 12.5px;
            font-weight: 700;
            padding: 10px 14px;
            border: 1.5px solid var(--border-color);
            border-radius: 12px;
            background: var(--card-color);
            color: var(--text-dark);
            cursor: pointer;
            text-align: left;
            transition: all 0.2s ease;
            outline: none;
            line-height: 1.4;
            flex-shrink: 0;
            word-break: keep-all;
        }

        button:hover {
            background-color: var(--hover-color);
            border-color: var(--accent-color);
        }

        .progress-container {
            width: 100%;
            height: 5px;
            background-color: #e2e8f0;
            border-radius: 999px;
            margin-top: 10px;
            margin-bottom: 10px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            width: 0%;
            background-color: var(--accent-color);
            border-radius: 999px;
            transition: width 0.3s ease;
        }

        .question-number {
            font-size: 11px;
            font-weight: 800;
            color: var(--accent-color);
            margin-bottom: 4px;
            text-align: center;
        }

        .question-text {
            font-size: 15px;
            font-weight: 800;
            line-height: 1.4;
            margin-bottom: 10px;
            text-align: center;
            color: var(--primary-color);
            word-break: keep-all;
        }

        /* Fully Centered Loading Area Layout */
        .loading-wrapper {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            width: 100%;
            height: 100%;
        }

        .spinner {
            width: 44px;
            height: 44px;
            border: 4px solid var(--light-blue);
            border-top: 4px solid var(--accent-color);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-bottom: 16px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Raised Up Results Profile UI Elements */
        .character-icon-box {
            font-size: 48px;
            text-align: center;
            margin-top: 5px;
            margin-bottom: 2px;
            animation: bounce 2s infinite alternate;
        }

        @keyframes bounce {
            0% { transform: translateY(0); }
            100% { transform: translateY(-6px); }
        }

        .result-badge {
            align-self: center;
            background-color: var(--light-blue);
            color: var(--accent-color);
            padding: 3px 10px;
            border-radius: 20px;
            font-size: 10px;
            font-weight: 800;
            margin-top: 5px;
            margin-bottom: 2px;
        }

        .result-title {
            font-size: 19px;
            font-weight: 800;
            text-align: center;
            color: var(--primary-color);
        }

        .result-mbti {
            font-size: 13px;
            font-weight: 700;
            color: var(--text-light);
            text-align: center;
            margin-bottom: 6px;
        }

        .result-scroll-area {
            flex: 1;
            overflow-y: auto;
            padding-right: 4px;
            display: flex;
            flex-direction: column;
            gap: 8px;
            margin-bottom: 8px;
        }

        .result-scroll-area::-webkit-scrollbar {
            width: 4px;
        }
        .result-scroll-area::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 4px;
        }

        .section-box {
            background: var(--light-blue);
            padding: 12px;
            border-radius: 12px;
            font-size: 12px;
            line-height: 1.5;
            color: #334155;
            border: 1px solid #e2e8f0;
        }

        .section-title {
            font-size: 12.5px;
            font-weight: 800;
            margin-bottom: 4px;
            display: flex;
            align-items: center;
            gap: 4px;
        }

        .title-strength { color: var(--success-color); }
        .title-weakness { color: var(--danger-color); }
        .title-chemistry { color: var(--primary-color); }

        .chem-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
            margin-top: 4px;
        }

        .chem-item {
            background: white;
            padding: 6px;
            border-radius: 8px;
            border: 1px solid #e2e8f0;
            text-align: center;
        }

        .chem-label {
            font-size: 10px;
            font-weight: 800;
            margin-bottom: 2px;
        }

        .chem-value {
            font-size: 12px;
            font-weight: 800;
        }

        .action-btn {
            text-align: center !important;
            background-color: var(--accent-color) !important;
            color: white !important;
            border: none !important;
            width: 100%;
            padding: 12px 0;
            border-radius: 12px;
            font-weight: 800;
        }
    </style>
</head>
<body>

    <div class="quiz-container">
        
        <!-- PAGE 0: PHILOSOPHY INTRO -->
        <div id="intro-page" class="page active">
            <div class="top-content">
                <div class="philosophy-title"><span>PHIL</span>osophy</div>
                <div class="philosophy-box">
                    이 테스트는 정답이 정해져 있는 시험이 아니에요!<br><br>
                    복잡한 회사 생활 속에서 내가 진짜 중요하게 생각하는 가치관과 나만의 일하는 스타일을 찾아가는 과정입니다. 깊게 고민하기보다 마음이 가는 선택지를 편하게 골라주세요. 😊
                </div>
            </div>
            <button class="action-btn" onclick="switchPage('intro-page', 'start-page')">테스트 시작하기</button>
        </div>
        
        <!-- PAGE 1: RANK SELECTION -->
        <div id="start-page" class="page">
            <div class="top-content">
                <h1>나의 직급을 선택해주세요</h1>
                <p class="subtitle">본인과 가장 가까운 위치를 골라주세요.</p>
            </div>
            <div class="btn-grid">
                <button onclick="startQuiz('사원')">사원 (Junior Associate)</button>
                <button onclick="startQuiz('선임')">선임 (Senior Associate)</button>
                <button onclick="startQuiz('책임')">책임 (Manager / Specialist)</button>
                <button onclick="startQuiz('팀장')">팀장 (Team Leader)</button>
                <button onclick="startQuiz('상무')">상무 (Executive / VP)</button>
            </div>
            <div></div>
        </div>

        <!-- PAGE 2: QUIZ INTERFACE -->
        <div id="quiz-page" class="page">
            <div class="top-content">
                <div class="progress-container">
                    <div id="progress-bar" class="progress-bar"></div>
                </div>
                <div id="question-num" class="question-number">Question 1 of 15</div>
                <div id="question-txt" class="question-text">질문 불러오는 중...</div>
            </div>
            <div class="btn-grid" id="answer-options">
                <!-- Choices rendered inside layout system safely -->
            </div>
        </div>

        <!-- PAGE 3: DELIBERATE DELAY -->
        <div id="loading-page" class="page">
            <div class="loading-wrapper">
                <div class="spinner"></div>
                <div id="loading-status" class="loading-text" style="font-weight: 800; color: var(--primary-color);">답변을 분석하고 있어요...</div>
                <p class="subtitle" style="margin-top: 8px;">나의 회사 생활 성향을 계산하는 중입니다.</p>
            </div>
        </div>

        <!-- PAGE 4: ARCHETYPE OUTCOME -->
        <div id="result-page" class="page">
            <div class="top-content">
                <div id="result-rank-tag" class="result-badge">결과 분석 완료</div>
                <div id="result-icon" class="character-icon-box">👔</div>
                <div id="result-title" class="result-title">나의 유형</div>
                <div id="result-mbti" class="result-mbti">MBTI TYPE</div>
            </div>
            
            <div class="result-scroll-area">
                <div id="result-desc" class="section-box">개요 로딩 중...</div>
                
                <div class="section-box">
                    <div class="section-title title-strength">💪 업무적 강점</div>
                    <div id="result-strengths">성향별 강점 로딩 중...</div>
                </div>

                <div class="section-box">
                    <div class="section-title title-weakness">⚠️ 주의할 약점</div>
                    <div id="result-weaknesses">성향별 주의점 로딩 중...</div>
                </div>

                <div class="section-box">
                    <div class="section-title title-chemistry">🤝 사내 협업 케미</div>
                    <div class="chem-grid">
                        <div class="chem-item" style="border-top: 3px solid var(--success-color);">
                            <div class="chem-label" style="color: var(--success-color);">최고의 파트너</div>
                            <div id="result-best-chem" class="chem-value">----</div>
                        </div>
                        <div class="chem-item" style="border-top: 3px solid var(--danger-color);">
                            <div class="chem-label" style="color: var(--danger-color);">조율 필요 파트너</div>
                            <div id="result-worst-chem" class="chem-value">----</div>
                        </div>
                    </div>
                </div>
            </div>

            <button class="action-btn" onclick="resetQuiz()">다시 테스트하기</button>
        </div>

    </div>

    <script>
        const questionDatabase = {
            "사원": [
                {
                    q: "출근했는데 능력치를 벗어난 수준의 과도한 업무가 쌓여있다면?",
                    options: [
                        { text: "우선순위와 기한을 리스트로 차분하게 정리해 하나씩 쳐낸다.", traits: ['J', 'T'] },
                        { text: "주변 동기나 선배들 상황을 살피며 조심스레 도움을 요청한다.", traits: ['E', 'F'] },
                        { text: "일단 손에 바로 잡히는 가장 쉽고 빠른 일부터 속도감 있게 끝낸다.", traits: ['P', 'S'] },
                        { text: "이 업무가 우리 팀 전체 방향에서 어떤 그림을 갖는지 의도를 본다.", traits: ['N', 'I'] }
                    ]
                },
                {
                    q: "열심히 준비한 기획서가 상세 피드백 없이 통째로 반려당했다면?",
                    options: [
                        { text: "선배들의 이전 기획서들을 분석해 내 문서의 오점을 찾아낸다.", traits: ['S', 'T'] },
                        { text: "기존 틀에서 벗어나 아예 새로운 시각과 아이디어로 재구상한다.", traits: ['N', 'P'] },
                        { text: "커피 타임을 빌려 팀장님의 진짜 의도가 무엇이었는지 조언을 구한다.", traits: ['E', 'F'] },
                        { text: "혼자 자리에 앉아 반려 이유를 조용히 곱씹으며 논리를 보완한다.", traits: ['I', 'J'] }
                    ]
                },
                {
                    q: "타 부서 동료가 메신저로 다소 딱딱하고 날카롭게 수정을 요구할 때?",
                    options: [
                        { text: "감정은 배제하고 상대방이 요청한 팩트만 확인해 깔끔히 피드백한다.", traits: ['T', 'I'] },
                        { text: "상대방이 마감 압박으로 예민할 수 있으니 친절하고 유연하게 맞춘다.", traits: ['F', 'P'] },
                        { text: "오해가 생기지 않도록 직접 전화를 걸거나 찾아가 시원하게 해결한다.", traits: ['E', 'S'] },
                        { text: "이런 불필요한 마찰이 왜 반복되는지 소통 포맷의 대안을 생각한다.", traits: ['N', 'J'] }
                    ]
                },
                {
                    q: "금요일 퇴근 직전, 다음 주 초 마감인 리서치 과제가 갑자기 내려온다면?",
                    options: [
                        { text: "주말에 푹 쉬기 위해 오늘 야근을 하더라도 무조건 끝내고 간다.", traits: ['J', 'S'] },
                        { text: "일단 퇴근하고 주말에 쉬면서 아이디어가 떠오를 때 틈틈이 모은다.", traits: ['P', 'N'] },
                        { text: "팀원들과 공유하며 혹시 비슷한 자료를 가진 사람이 있는지 수소문한다.", traits: ['E', 'F'] },
                        { text: "조용히 퇴근한 뒤 주말에 방해받지 않는 시간에 혼자 집중해서 짠다.", traits: ['I', 'T'] }
                    ]
                },
                {
                    q: "팀 회의 중에 의견이 모이지 않고 대화가 완전히 막혀버렸을 때 나는?",
                    options: [
                        { text: "기존에 검증된 성공 사례나 데이터 위주로 안전한 차선책을 꺼낸다.", traits: ['S', 'J'] },
                        { text: "현실 제약은 던져두고 판을 전환할 기발하고 파격적인 아이디어를 제안한다.", traits: ['N', 'P'] },
                        { text: "무거워진 분위기를 풀기 위해 가벼운 리액션이나 유머로 텐션을 올린다.", traits: ['E', 'F'] },
                        { text: "동료들이 주고받는 대화를 경청하며 머릿속으로 핵심 논리를 정리한다.", traits: ['I', 'T'] }
                    ]
                }
            ]
        };

        const genericTemplate = [
            { q: "예상치 못한 돌발 변수로 프로젝트 일정이 지연될 것 같을 때?", options: [
                { text: "지연 타임라인과 리스크를 수치화해 상사에게 빠르게 보고한다.", traits: ['T', 'J'] },
                { text: "담당자들을 신속히 소집해 함께 해결할 돌파구를 모색한다.", traits: ['E', 'N'] },
                { text: "보고를 고민하기보다 일단 터진 현장 수습에 온 몸을 던진다.", traits: ['S', 'P'] },
                { text: "팀원들이 자책하지 않도록 멘탈을 다독이며 분위기를 추스른다.", traits: ['F', 'I'] }
            ]},
            { q: "새로운 사내 프로그램이나 워크숍을 기획할 때 중시하는 것은?", options: [
                { text: "참여 데이터 기반으로 빈틈없이 꼼꼼하게 짜인 일정표 구축", traits: ['J', 'S'] },
                { text: "기존 틀을 깨고 신선한 자극과 영감을 얻어 가도록 세션 연출", traits: ['N', 'P'] },
                { text: "구성원들이 자연스레 섞여 끈끈한 유대감을 다질 수 있게 유도", traits: ['E', 'F'] },
                { text: "불필요한 단체 이벤트를 빼고 개인 휴식을 온전히 보장하는 것", traits: ['I', 'T'] }
            ]},
            { q: "동료가 일처리를 돕는 업무 자동화 툴을 공유해 주었다면?", options: [
                { text: "작동 원리가 무엇인지, 오류 확률은 없는지 논리적으로 검증한다.", traits: ['T', 'I'] },
                { text: "실제 업무 반영 시 보안이나 사내 규정상 문제가 없는지 살핀다.", traits: ['S', 'J'] },
                { text: "툴을 짜준 동료에게 고마워하며 주변 팀원들에게도 널리 알린다.", traits: ['E', 'F'] },
                { text: "신기해하며 내 다른 업무 영역에도 적용해 볼 방안을 상상한다.", traits: ['N', 'P'] }
            ]},
            { q: "인사 고과나 상반기 평가 결과가 내 노력에 비해 아쉽게 나왔다면?", options: [
                { text: "증빙할 수 있는 실적 지표를 챙겨 정량적 이의신청을 진행한다.", traits: ['T', 'J'] },
                { text: "회사의 전반적인 기류나 평가권자의 의중 매커니즘을 짚어본다.", traits: ['N', 'I'] },
                { text: "마음 맞는 동료와 시원하게 한잔하며 훌훌 털고 다음을 준비한다.", traits: ['F', 'P'] },
                { text: "실무에서 누락된 아웃풋이 무엇이었는지 객관적으로 복기한다.", traits: ['S', 'E'] }
            ]},
            { q: "함께 오랫동안 일할 외주 협력사를 고를 때 나만의 핵심 기준은?", options: [
                { text: "포트폴리오가 완벽하고 인프라 체계가 확립된 검증 완료된 기업", traits: ['S', 'J'] },
                { text: "비즈니스 비전이 맞고 감각적인 제안이 뛰어난 트렌디한 기업", traits: ['N', 'P'] },
                { text: "소통 피드백 속도가 빠르고 실무 대화가 유연하게 풀리는 기업", traits: ['E', 'F'] },
                { text: "가이드라인과 계약 조항을 완벽하게 엄수하는 독립적 기업", traits: ['I', 'T'] }
            ]},
            { q: "발표 직전 프레젠테이션 장표에서 심각한 오탈자를 발견했다면?", options: [
                { text: "발견 즉시 정정 수치를 깔끔하게 요약해 참석자들에게 전달한다.", traits: ['T', 'S'] },
                { text: "당황을 지우고 내 말솜씨와 스토리텔링 임기응변으로 가린다.", traits: ['P', 'N'] },
                { text: "동료에게 수정을 부탁하고 내 발표 포커스페이스 유지를 우선한다.", traits: ['E', 'F'] },
                { text: "회의 직전까지 집중력을 발휘해 내 손으로 직접 수정한다.", traits: ['I', 'J'] }
            ]},
            { q: "큰 프로젝트를 성공리에 마무리 지었을 때 가장 큰 보람은?", options: [
                { text: "초기에 정밀 조율했던 타깃 수치(KPI)를 완전히 달성했을 때", traits: ['T', 'J'] },
                { text: "남들이 가보지 못한 가치 있는 아이디어를 시장에 최초 증명 시", traits: ['N', 'S'] },
                { text: "팀원들과 함께 고생하며 끈끈한 인간적 신뢰가 두터워졌을 때", traits: ['F', 'E'] },
                { text: "어려운 실무 허들을 내 역량으로 극복해 성장을 완수했을 때", traits: ['I', 'P'] }
            ]},
            { q: "부서 인사이동으로 완전히 생소한 직무 환경을 만났다면?", options: [
                { text: "인수인계 아카이브와 매뉴얼을 찾아 조용히 깊이 정독한다.", traits: ['I', 'T'] },
                { text: "핵심 담당자들을 찾아가 인사하며 실무 네트워크를 먼저 뚫는다.", traits: ['E', 'F'] },
                { text: "기본적인 양식 흐름만 파악한 채 실전 필드에 부딪치며 파악한다.", traits: ['S', 'J'] },
                { text: "기존 관행을 따르기보단 타 산업 혁신 케이스의 이식을 구상한다.", traits: ['N', 'P'] }
            ]},
            { q: "팀 내부에서 세대 간 가치관 차이로 소통 마찰이 심해졌을 때?", options: [
                { text: "자연스러운 티타임 자리를 만들어 정서적 벽을 먼저 무너뜨린다.", traits: ['F', 'E'] },
                { text: "업무에서 지켜야 할 명확한 상호 R&R 그라운드 룰을 제정한다.", traits: ['T', 'J'] },
                { text: "누구의 편도 들지 않고 양측 맥락을 관찰하며 융화를 기다린다.", traits: ['I', 'P'] },
                { text: "협업 커뮤니케이션 공유 시스템을 통일해 오해 소지를 차단한다.", traits: ['S', 'N'] }
            ]},
            { q: "냉정한 오피스 생태계에서 내가 생각하는 진짜 일잘러의 기준은?", options: [
                { text: "흔들리지 않는 냉철한 이성과 데이터로 확실한 결과를 꺼내는 사람", traits: ['T', 'J'] },
                { text: "남다른 시각으로 지루한 정체 국면을 파괴하고 혁신 루트를 뚫는 사람", traits: ['N', 'P'] },
                { text: "팀을 배려하고 소통을 조율해 전체 효율을 배가시키는 매력적인 사람", traits: ['F', 'E'] },
                { text: "묵묵히 자기 위치에서 고도의 전문성을 수행하며 신뢰를 주는 사람", traits: ['S', 'I'] }
            ]}
        ];

        function normalizeDatabases() {
            const ranks = ["사원", "선임", "책임", "팀장", "상무"];
            ranks.forEach(rank => {
                if (!questionDatabase[rank]) questionDatabase[rank] = [];
                const baseLen = questionDatabase[rank].length;
                const needed = 15 - baseLen;
                for(let i=0; i<needed; i++) {
                    questionDatabase[rank].push(genericTemplate[i % genericTemplate.length]);
                }
            });
        }
        normalizeDatabases();

        const archetypeRegistry = {
            "INTJ": { title: "전략적 아키텍트", icon: "🧠", desc: "철저한 데이터와 타당성을 중시하는 브레인 리더형 인재입니다.", strengths: "거시적 구조 설계력, 위기 속 흔들리지 않는 냉철한 이성적 판단.", weaknesses: "타인의 정서적 피로감 간과 가능성, 지나치게 엄격한 기준.", best: "ENFP", worst: "ESFJ" },
            "INTP": { title: "시스템 분석가", icon: "🔬", desc: "복잡한 난제를 단순화하고 구조의 허점을 완벽히 찾아냅니다.", strengths: "독창적인 논리력, 선입견 없는 유연한 아이디어 분석력.", weaknesses: "반복적인 단순 실무 처리 지연, 다소 차갑게 보이는 소통 방식.", best: "ENTJ", worst: "ESFP" },
            "ENTJ": { title: "마켓 커맨더", icon: "🦅", desc: "명확한 비전을 행해 팀을 강력히 드라이브하는 완벽 지향 리더.", strengths: "탁월한 조직 장악력과 추진력, 과감한 자원 조율과 실행력.", weaknesses: "과속으로 인한 팀원 이탈 유발, 디테일한 수작업 소홀.", best: "INTP", worst: "ISFP" },
            "ENTP": { title: "패러다임 디스럽터", icon: "⚡", desc: "낡은 관행을 시원하게 무너뜨리며 대안을 제시하는 아이디어 뱅크.", strengths: "막강한 임기응변, 새로운 비즈니스 기회 확보력.", weaknesses: "기획 대비 마무리가 다소 약함, 잦은 시스템 변덕.", best: "INFJ", worst: "ISFJ" },
            "INFJ": { title: "인사이트 멘토", icon: "🔮", desc: "조직의 비전과 동료들의 잠재력을 예리하게 연결해내는 가치 추구자.", strengths: "심도 깊은 경청 및 소통력, 조직 내 갈등 중재 능력.", weaknesses: "속마음을 잘 공유하지 않아 생기는 오해, 비판에 상처받음.", best: "ENTP", worst: "ESTP" },
            "INFP": { title: "비전 크리에이터", icon: "🎨", desc: "자율성과 정서적 유대를 소중히 여기는 오피스 아티스트.", strengths: "구성원 가치 존중, 창의적이고 감성적인 기획 스토리텔링.", weaknesses: "냉정한 피드백 수용 시 멘탈 저하, 행정적 디테일 기피.", best: "ENFJ", worst: "ESTJ" },
            "ENFJ": { title: "시너지 리더", icon: "🌱", desc: "팀을 따뜻하게 포용하여 같은 목표로 묶어주는 인간 허브.", strengths: "뛰어난 카리스마와 동기부여력, 타 부서 협업 조율가.", weaknesses: "갈등을 회피하려다 골이 깊어짐, 지나친 타인 의존.", best: "INFP", worst: "ISTP" },
            "ENFP": { title: "스파크 디렉터", icon: "🎈", desc: "경직된 조직 무드에 끊임없는 영감을 채워주는 활력 아이콘.", strengths: "적극적인 친화력과 네트워킹, 탁월한 트렌드 센스.", weaknesses: "일정 및 마감 기한 관리 취약, 쉽게 질리는 업무 집중력.", best: "INTJ", worst: "ISTJ" },
            "ISTJ": { title: "신뢰의 앵커", icon: "⚓", desc: "무결점 데이터와 규칙 준수로 시스템을 지탱하는 오피스 척추.", strengths: "압도적인 책임감, 예측 가능한 안정성과 명확한 매뉴얼 수행.", weaknesses: "급작스러운 업무 변화에 스트레스 취약, 유연성 부족.", best: "ESFP", worst: "ENFP" },
            "ISFJ": { title: "보이지 않는 수호자", icon: "🛡️", desc: "성실하게 내 자리를 지키며 동료들의 고충을 서포트하는 조력자.", strengths: "뛰어난 디테일 기억력, 안전하고 꼼꼼한 백오피스 관리.", weaknesses: "과도한 업무 거절 거부로 인한 번아웃, 과감한 혁신 기피.", best: "ESTP", worst: "ENTP" },
            "ESTJ": { title: "실무 오퍼레이터", icon: "📊", desc: "명확한 타임라인 조율로 실질적인 결과를 창출하는 성과 가속기.", strengths: "논리적이고 체계적인 일정 통제, 명확한 R&R 기반의 추진력.", weaknesses: "과정보다 결과 위주의 판단, 융통성 없는 규정 들이대기.", best: "ISFP", worst: "INFP" },
            "ESFJ": { title: "하모니 오케스트레이터", icon: "🎻", desc: "사내 평판และ 동료 관계를 가장 조화롭게 가꾸는 소통 마에스트로.", strengths: "풍부한 협업 조율 센스, 구성원의 세심한 정서 케어.", weaknesses: "객관적인 의사결정 시 우유부단함, 동료 평가에 과민반응.", best: "ISFP", worst: "INTJ" },
            "ISTP": { title: "위기 수습 리얼리스트", icon: "🔧", desc: "불필요한 대화를 걷어내고 실무 필드에서 문제를 치워내는 해결사.", strengths: "탁월한 도구/기술 활용력, 긴급 돌발 장애 해결력.", weaknesses: "조직 내 단체 행사 참여 회피, 다소 단답형인 소통 성향.", best: "ESFJ", worst: "ENFJ" },
            "ISFP": { title: "아티잔 플레이어", icon: "🍃", desc: "압박에 굴하지 않고 나만의 퀄리티를 유지해내는 무던한 실력파.", strengths: "상황에 유연하게 대처하는 조화로움, 미적·감성적 직관력.", weaknesses: "장기적인 커리어 로드맵 설계 약화, 적극적 의견 개진 기피.", best: "ESTJ", worst: "ENTJ" },
            "ESTP": { title: "공격적 스트라이커", icon: "🎯", desc: "이론에 갇히지 않고 직접 필드에서 계약을 따내는 행동주의 인재.", strengths: "탁월한 실무 타협 및 영업 감각, 현장 중심 순발력.", weaknesses: "문서화 및 행정 절차 소홀, 리스크 계산 없는 돌격 성향.", best: "ISFJ", worst: "INFJ" },
            "ESFP": { title: "액티브 메이커", icon: "✨", desc: "일터의 분위기를 유쾌하게 리드하여 시너지를 만드는 오피스 스타.", strengths: "오픈 마인드의 협동심 유발, 즉흥적 환경 적응 능력.", weaknesses: "이성적이고 차가운 피드백에 쉽게 동요, 장기 기획 약세.", best: "ISTJ", worst: "INTP" }
        };

        let currentRank = "";
        let currentQuestionIdx = 0;
        let traitScores = { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 };

        function switchPage(fromPageId, toPageId, callback) {
            const fromPage = document.getElementById(fromPageId);
            const toPage = document.getElementById(toPageId);

            fromPage.classList.add('exit');
            setTimeout(() => {
                fromPage.classList.remove('active', 'exit');
                fromPage.style.display = 'none';
                
                toPage.style.display = 'flex';
                void toPage.offsetWidth; 
                toPage.classList.add('active');
                if (callback) callback();
            }, 250);
        }

        function startQuiz(rank) {
            currentRank = rank;
            currentQuestionIdx = 0;
            traitScores = { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 };
            
            switchPage('start-page', 'quiz-page', () => {
                displayQuestion();
            });
        }

        function displayQuestion() {
            const questions = questionDatabase[currentRank];
            const currentQuestion = questions[currentQuestionIdx];

            document.getElementById('question-num').innerText = `Question ${currentQuestionIdx + 1} of 15`;
            document.getElementById('question-txt').innerText = currentQuestion.q;
            
            const optionsContainer = document.getElementById('answer-options');
            optionsContainer.innerHTML = "";
            optionsContainer.scrollTop = 0;
            
            currentQuestion.options.forEach(opt => {
                const btn = document.createElement('button');
                btn.innerText = opt.text;
                btn.onclick = () => submitAnswer(opt.traits);
                optionsContainer.appendChild(btn);
            });

            const progressPercent = ((currentQuestionIdx + 1) / 15) * 100;
            document.getElementById('progress-bar').style.width = `${progressPercent}%`;
        }

        function submitAnswer(selectedTraits) {
            selectedTraits.forEach(trait => {
                traitScores[trait]++;
            });

            const questions = questionDatabase[currentRank];
            
            if (currentQuestionIdx < questions.length - 1) {
                const quizPage = document.getElementById('quiz-page');
                quizPage.classList.add('exit');
                
                setTimeout(() => {
                    currentQuestionIdx++;
                    displayQuestion();
                    quizPage.classList.remove('exit');
                }, 220);
            } else {
                switchPage('quiz-page', 'loading-page', () => {
                    runLoadingSequence();
                });
            }
        }

        function runLoadingSequence() {
            const statusText = document.getElementById('loading-status');
            
            setTimeout(() => {
                statusText.innerText = "종합 케미 매트릭스 산출 중...";
            }, 950);

            setTimeout(() => {
                switchPage('loading-page', 'result-page', () => {
                    renderFinalResult();
                });
            }, 2000); 
        }

        function renderFinalResult() {
            document.getElementById('result-rank-tag').innerText = `${currentRank} 직급 종합 리포트`;
            
            let mbtiResult = "";
            mbtiResult += (traitScores['E'] >= traitScores['I']) ? "E" : "I";
            mbtiResult += (traitScores['S'] >= traitScores['N']) ? "S" : "N";
            mbtiResult += (traitScores['T'] >= traitScores['F']) ? "T" : "F";
            mbtiResult += (traitScores['J'] >= traitScores['P']) ? "J" : "P";

            const data = archetypeRegistry[mbtiResult] || archetypeRegistry["INTJ"];
            
            document.getElementById('result-icon').innerText = data.icon;
            document.getElementById('result-title').innerText = data.title;
            document.getElementById('result-mbti').innerText = `[ 회사 생활 성향 : ${mbtiResult} ]`;
            document.getElementById('result-desc').innerText = data.desc;
            
            document.getElementById('result-strengths').innerText = data.strengths;
            document.getElementById('result-weaknesses').innerText = data.weaknesses;
            document.getElementById('result-best-chem').innerText = data.best;
            document.getElementById('result-worst-chem').innerText = data.worst;
            
            document.querySelector('.result-scroll-area').scrollTop = 0;
        }

        function resetQuiz() {
            switchPage('result-page', 'intro-page');
        }
    </script>
</body>
</html>
