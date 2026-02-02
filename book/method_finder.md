# Method Finder

어떤 인과추론 방법을 사용해야 할지 모르겠나요?
두 가지 방법으로 적절한 방법론을 찾아보세요.

---

## 1. Decision Tree

질문에 답하며 단계별로 방법론을 좁혀갑니다.

```{raw} html
<style>
.decision-tree {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    max-width: 600px;
    margin: 20px 0;
}
.dt-question {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 20px;
    border-radius: 12px;
    margin: 10px 0;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}
.dt-question h4 {
    margin: 0 0 15px 0;
    font-size: 1.1em;
}
.dt-buttons {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}
.dt-btn {
    padding: 10px 24px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-size: 0.95em;
    font-weight: 500;
    transition: all 0.2s;
}
.dt-btn-yes {
    background: #10b981;
    color: white;
}
.dt-btn-no {
    background: #ef4444;
    color: white;
}
.dt-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}
.dt-result {
    background: linear-gradient(135deg, #10b981 0%, #059669 100%);
    color: white;
    padding: 24px;
    border-radius: 12px;
    margin: 10px 0;
    display: none;
}
.dt-result h4 {
    margin: 0 0 10px 0;
    font-size: 1.2em;
}
.dt-result a {
    color: white;
    text-decoration: underline;
    font-weight: bold;
}
.dt-result p {
    margin: 10px 0;
    opacity: 0.9;
}
.dt-reset {
    background: #6b7280;
    color: white;
    padding: 8px 16px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    margin-top: 10px;
}
.dt-hidden { display: none !important; }
.dt-path {
    background: #f3f4f6;
    padding: 10px 15px;
    border-radius: 8px;
    margin: 10px 0;
    font-size: 0.9em;
    color: #374151;
}
</style>

<div class="decision-tree">
    <div id="dt-path" class="dt-path dt-hidden">경로: <span id="dt-path-text"></span></div>

    <div id="q1" class="dt-question">
        <h4>Q1. 데이터가 무작위 실험(RCT)에서 나왔나요?</h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q1', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q1', false)">아니오</button>
        </div>
    </div>

    <div id="q2-rct" class="dt-question dt-hidden">
        <h4>Q2. Encouragement Design인가요?<br><small>(처치 권유만 무작위, 실제 처치는 선택)</small></h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q2-rct', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q2-rct', false)">아니오</button>
        </div>
    </div>

    <div id="q3-rct" class="dt-question dt-hidden">
        <h4>Q3. 사전 공변량(pretreatment variables)이 있나요?</h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q3-rct', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q3-rct', false)">아니오</button>
        </div>
    </div>

    <div id="q2-obs" class="dt-question dt-hidden">
        <h4>Q2. 처치(treatment)가 이진(binary)인가요?</h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q2-obs', true)">예 (0/1)</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q2-obs', false)">아니오 (연속)</button>
        </div>
    </div>

    <div id="q3-temporal" class="dt-question dt-hidden">
        <h4>Q3. 시간 정보가 있나요?<br><small>(패널 데이터, 처치 전후 시점)</small></h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q3-temporal', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q3-temporal', false)">아니오</button>
        </div>
    </div>

    <div id="q4-staggered" class="dt-question dt-hidden">
        <h4>Q4. Staggered adoption인가요?<br><small>(처치 시점이 유닛마다 다름)</small></h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q4-staggered', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q4-staggered', false)">아니오</button>
        </div>
    </div>

    <div id="q4-running" class="dt-question dt-hidden">
        <h4>Q4. Running variable이 있나요?<br><small>(cutoff 기준으로 처치 여부 결정)</small></h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q4-running', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q4-running', false)">아니오</button>
        </div>
    </div>

    <div id="q5-backdoor" class="dt-question dt-hidden">
        <h4>Q5. 백도어 기준이 만족되나요?<br><small>(관측된 공변량으로 교란 통제 가능)</small></h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q5-backdoor', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q5-backdoor', false)">아니오</button>
        </div>
    </div>

    <div id="q6-balance" class="dt-question dt-hidden">
        <h4>Q6. 공변량이 균형 잡혀있나요?<br><small>(처치군/대조군 간 공변량 분포 유사)</small></h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q6-balance', true)">예 (균형)</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q6-balance', false)">아니오 (불균형)</button>
        </div>
    </div>

    <div id="q6-iv" class="dt-question dt-hidden">
        <h4>Q6. 유효한 도구 변수(IV)가 있나요?</h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q6-iv', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q6-iv', false)">아니오</button>
        </div>
    </div>

    <div id="q7-frontdoor" class="dt-question dt-hidden">
        <h4>Q7. 프론트도어 기준이 만족되나요?<br><small>(매개 변수를 통한 식별 가능)</small></h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q7-frontdoor', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q7-frontdoor', false)">아니오</button>
        </div>
    </div>

    <div id="q3-continuous" class="dt-question dt-hidden">
        <h4>Q3. 백도어 기준이 만족되나요?</h4>
        <div class="dt-buttons">
            <button class="dt-btn dt-btn-yes" onclick="dtAnswer('q3-continuous', true)">예</button>
            <button class="dt-btn dt-btn-no" onclick="dtAnswer('q3-continuous', false)">아니오</button>
        </div>
    </div>

    <!-- Results -->
    <div id="result-iv" class="dt-result">
        <h4>→ Instrumental Variables (IV)</h4>
        <p>도구 변수를 활용해 LATE를 추정합니다.</p>
        <a href="part1/iv.ipynb">IV 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-ols" class="dt-result">
        <h4>→ OLS with Controls</h4>
        <p>공변량을 통제한 OLS로 ATE를 추정합니다.</p>
        <a href="part1/ols.ipynb">OLS 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-dim" class="dt-result">
        <h4>→ Difference in Means</h4>
        <p>단순 평균 차이로 ATE를 추정합니다. (별도 노트북 없음)</p>
        <button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-sdid" class="dt-result">
        <h4>→ Synthetic DiD (SDID)</h4>
        <p>Staggered adoption에 적합한 합성 이중차분법입니다.</p>
        <a href="part1/sdid.ipynb">SDID 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-did" class="dt-result">
        <h4>→ Difference-in-Differences (DiD)</h4>
        <p>평행 추세 가정 하에 ATE를 추정합니다.</p>
        <a href="part1/did.ipynb">DiD 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-rdd" class="dt-result">
        <h4>→ Regression Discontinuity Design (RDD)</h4>
        <p>cutoff 근처에서 LATE를 추정합니다.</p>
        <a href="part1/rdd.ipynb">RDD 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-ipw" class="dt-result">
        <h4>→ Inverse Probability Weighting (IPW)</h4>
        <p>성향 점수 기반 가중치로 ATE를 추정합니다.</p>
        <a href="part1/ipw_psm.ipynb">IPW/PSM 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-psm" class="dt-result">
        <h4>→ Propensity Score Matching (PSM)</h4>
        <p>성향 점수 매칭으로 공변량 균형을 맞춘 후 ATE를 추정합니다.</p>
        <a href="part1/ipw_psm.ipynb">IPW/PSM 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-frontdoor" class="dt-result">
        <h4>→ Frontdoor Criterion</h4>
        <p>매개 변수를 통해 인과 효과를 식별합니다.</p>
        <a href="examples/frontdoor_criterion.ipynb">Frontdoor 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-dml" class="dt-result">
        <h4>→ Double Machine Learning (DML)</h4>
        <p>연속 처치에 대해 ML 기반으로 ATE/CATE를 추정합니다.</p>
        <a href="part2/dml.ipynb">DML 노트북 바로가기 →</a>
        <br><button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>

    <div id="result-na" class="dt-result" style="background: linear-gradient(135deg, #6b7280 0%, #4b5563 100%);">
        <h4>⚠️ 식별 불가</h4>
        <p>현재 조건으로는 인과 효과를 식별할 수 없습니다.<br>
        추가적인 가정이나 데이터가 필요합니다.</p>
        <button class="dt-reset" onclick="dtReset()">다시 시작</button>
    </div>
</div>

<script>
let dtPathList = [];

function dtAnswer(qid, answer) {
    document.getElementById(qid).classList.add('dt-hidden');

    const pathMap = {
        'q1': answer ? 'RCT 데이터' : '관측 데이터',
        'q2-rct': answer ? 'Encouragement' : '완전 무작위',
        'q3-rct': answer ? '공변량 있음' : '공변량 없음',
        'q2-obs': answer ? '이진 처치' : '연속 처치',
        'q3-temporal': answer ? '시간 정보 있음' : '횡단면',
        'q4-staggered': answer ? 'Staggered' : '동시 처치',
        'q4-running': answer ? 'Running var 있음' : 'Running var 없음',
        'q5-backdoor': answer ? '백도어 만족' : '백도어 불만족',
        'q6-balance': answer ? '균형' : '불균형',
        'q6-iv': answer ? 'IV 있음' : 'IV 없음',
        'q7-frontdoor': answer ? '프론트도어 만족' : '프론트도어 불만족',
        'q3-continuous': answer ? '백도어 만족' : '백도어 불만족'
    };

    if (pathMap[qid]) {
        dtPathList.push(pathMap[qid]);
        document.getElementById('dt-path').classList.remove('dt-hidden');
        document.getElementById('dt-path-text').textContent = dtPathList.join(' → ');
    }

    // Decision logic
    if (qid === 'q1') {
        if (answer) {
            document.getElementById('q2-rct').classList.remove('dt-hidden');
        } else {
            document.getElementById('q2-obs').classList.remove('dt-hidden');
        }
    } else if (qid === 'q2-rct') {
        if (answer) {
            showResult('result-iv');
        } else {
            document.getElementById('q3-rct').classList.remove('dt-hidden');
        }
    } else if (qid === 'q3-rct') {
        showResult(answer ? 'result-ols' : 'result-dim');
    } else if (qid === 'q2-obs') {
        if (answer) {
            document.getElementById('q3-temporal').classList.remove('dt-hidden');
        } else {
            document.getElementById('q3-continuous').classList.remove('dt-hidden');
        }
    } else if (qid === 'q3-temporal') {
        if (answer) {
            document.getElementById('q4-staggered').classList.remove('dt-hidden');
        } else {
            document.getElementById('q4-running').classList.remove('dt-hidden');
        }
    } else if (qid === 'q4-staggered') {
        showResult(answer ? 'result-sdid' : 'result-did');
    } else if (qid === 'q4-running') {
        if (answer) {
            showResult('result-rdd');
        } else {
            document.getElementById('q5-backdoor').classList.remove('dt-hidden');
        }
    } else if (qid === 'q5-backdoor') {
        if (answer) {
            document.getElementById('q6-balance').classList.remove('dt-hidden');
        } else {
            document.getElementById('q6-iv').classList.remove('dt-hidden');
        }
    } else if (qid === 'q6-balance') {
        showResult(answer ? 'result-ipw' : 'result-psm');
    } else if (qid === 'q6-iv') {
        if (answer) {
            showResult('result-iv');
        } else {
            document.getElementById('q7-frontdoor').classList.remove('dt-hidden');
        }
    } else if (qid === 'q7-frontdoor') {
        showResult(answer ? 'result-frontdoor' : 'result-na');
    } else if (qid === 'q3-continuous') {
        showResult(answer ? 'result-dml' : 'result-na');
    }
}

function showResult(resultId) {
    document.getElementById(resultId).style.display = 'block';
}

function dtReset() {
    dtPathList = [];
    document.querySelectorAll('.dt-question').forEach(el => el.classList.add('dt-hidden'));
    document.querySelectorAll('.dt-result').forEach(el => el.style.display = 'none');
    document.getElementById('dt-path').classList.add('dt-hidden');
    document.getElementById('q1').classList.remove('dt-hidden');
}
</script>
```

---

## 2. 태그 기반 필터

조건을 선택하면 해당하는 노트북만 표시됩니다.

```{raw} html
<style>
.tag-filter {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    margin: 20px 0;
}
.filter-section {
    margin: 15px 0;
    padding: 15px;
    background: #f8fafc;
    border-radius: 10px;
}
.filter-section h4 {
    margin: 0 0 10px 0;
    color: #334155;
    font-size: 0.95em;
}
.filter-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
}
.filter-tag {
    padding: 6px 14px;
    border: 2px solid #e2e8f0;
    border-radius: 20px;
    cursor: pointer;
    font-size: 0.85em;
    transition: all 0.2s;
    background: white;
}
.filter-tag:hover {
    border-color: #667eea;
    background: #f0f4ff;
}
.filter-tag.active {
    background: #667eea;
    color: white;
    border-color: #667eea;
}
.notebook-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 15px;
    margin-top: 20px;
}
.notebook-card {
    background: white;
    border: 1px solid #e2e8f0;
    border-radius: 10px;
    padding: 18px;
    transition: all 0.2s;
}
.notebook-card:hover {
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    transform: translateY(-2px);
}
.notebook-card.hidden {
    display: none;
}
.notebook-card h5 {
    margin: 0 0 8px 0;
    font-size: 1em;
}
.notebook-card h5 a {
    color: #1e40af;
    text-decoration: none;
}
.notebook-card h5 a:hover {
    text-decoration: underline;
}
.notebook-card p {
    margin: 0 0 10px 0;
    font-size: 0.85em;
    color: #64748b;
}
.notebook-card .tags {
    display: flex;
    flex-wrap: wrap;
    gap: 4px;
}
.notebook-card .tag {
    font-size: 0.7em;
    padding: 2px 8px;
    background: #f1f5f9;
    border-radius: 10px;
    color: #475569;
}
.filter-reset {
    background: #6b7280;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 6px;
    cursor: pointer;
    font-size: 0.85em;
    margin-top: 10px;
}
.result-count {
    margin: 15px 0;
    padding: 10px 15px;
    background: #ecfdf5;
    border-radius: 8px;
    color: #065f46;
    font-size: 0.9em;
}
</style>

<div class="tag-filter">
    <div class="filter-section">
        <h4>추정 대상 (Estimand)</h4>
        <div class="filter-tags">
            <span class="filter-tag" data-category="estimand" data-value="ate" onclick="toggleFilter(this)">ATE</span>
            <span class="filter-tag" data-category="estimand" data-value="late" onclick="toggleFilter(this)">LATE</span>
            <span class="filter-tag" data-category="estimand" data-value="cate" onclick="toggleFilter(this)">CATE</span>
            <span class="filter-tag" data-category="estimand" data-value="policy" onclick="toggleFilter(this)">Policy</span>
        </div>
    </div>

    <div class="filter-section">
        <h4>데이터 구조 (Data)</h4>
        <div class="filter-tags">
            <span class="filter-tag" data-category="data" data-value="cross-sectional" onclick="toggleFilter(this)">횡단면</span>
            <span class="filter-tag" data-category="data" data-value="panel" onclick="toggleFilter(this)">패널</span>
            <span class="filter-tag" data-category="data" data-value="longitudinal" onclick="toggleFilter(this)">종단</span>
            <span class="filter-tag" data-category="data" data-value="time-series" onclick="toggleFilter(this)">시계열</span>
        </div>
    </div>

    <div class="filter-section">
        <h4>처치 유형 (Treatment)</h4>
        <div class="filter-tags">
            <span class="filter-tag" data-category="treatment" data-value="binary" onclick="toggleFilter(this)">이진</span>
            <span class="filter-tag" data-category="treatment" data-value="continuous" onclick="toggleFilter(this)">연속</span>
            <span class="filter-tag" data-category="treatment" data-value="time-varying" onclick="toggleFilter(this)">시변</span>
        </div>
    </div>

    <div class="filter-section">
        <h4>식별 전략 (Identification)</h4>
        <div class="filter-tags">
            <span class="filter-tag" data-category="identification" data-value="selection-on-observables" onclick="toggleFilter(this)">Selection on Obs.</span>
            <span class="filter-tag" data-category="identification" data-value="design-based" onclick="toggleFilter(this)">Design-based</span>
            <span class="filter-tag" data-category="identification" data-value="backdoor" onclick="toggleFilter(this)">Backdoor</span>
            <span class="filter-tag" data-category="identification" data-value="frontdoor" onclick="toggleFilter(this)">Frontdoor</span>
        </div>
    </div>

    <button class="filter-reset" onclick="resetFilters()">필터 초기화</button>

    <div id="result-count" class="result-count">전체 19개 노트북</div>

    <div class="notebook-grid" id="notebook-grid">
        <!-- Part 1 -->
        <div class="notebook-card" data-estimand="ate" data-data="panel,cross-sectional" data-treatment="binary" data-identification="selection-on-observables">
            <h5><a href="part1/ols.ipynb">OLS + Fixed Effects</a></h5>
            <p>controls와 panel fixed effects를 포함한 OLS로 ATE 추정</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">패널</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="ate" data-data="cross-sectional" data-treatment="binary" data-identification="selection-on-observables">
            <h5><a href="part1/ipw_psm.ipynb">IPW & PSM</a></h5>
            <p>성향 점수 기반 IPW, PSM, AIPW로 ATE 추정</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="ate" data-data="panel" data-treatment="binary" data-identification="design-based">
            <h5><a href="part1/did.ipynb">Difference-in-Differences</a></h5>
            <p>패널 데이터에서 평행 추세 가정 하에 ATE 추정</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">패널</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="ate" data-data="time-series,panel" data-treatment="binary" data-identification="design-based">
            <h5><a href="part1/sc.ipynb">Synthetic Control</a></h5>
            <p>가중 평균으로 합성된 대조군을 사용해 ATE 추정</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">시계열</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="ate" data-data="panel" data-treatment="binary" data-identification="design-based">
            <h5><a href="part1/sdid.ipynb">Synthetic DiD</a></h5>
            <p>SC와 DiD를 결합한 방법으로 ATE 추정</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">패널</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="late" data-data="cross-sectional" data-treatment="binary" data-identification="design-based">
            <h5><a href="part1/iv.ipynb">Instrumental Variables</a></h5>
            <p>도구 변수를 활용한 LATE 추정</p>
            <div class="tags">
                <span class="tag">LATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="late" data-data="cross-sectional" data-treatment="binary" data-identification="design-based">
            <h5><a href="part1/rdd.ipynb">Regression Discontinuity</a></h5>
            <p>cutoff 기준 처치 할당 시 LATE 추정</p>
            <div class="tags">
                <span class="tag">LATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <!-- Part 2 -->
        <div class="notebook-card" data-estimand="ate,cate" data-data="cross-sectional" data-treatment="binary,continuous" data-identification="selection-on-observables">
            <h5><a href="part2/dml.ipynb">Double Machine Learning</a></h5>
            <p>cross-fitting을 활용한 ATE/CATE 추정</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">CATE</span>
                <span class="tag">횡단면</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="cate" data-data="cross-sectional" data-treatment="binary" data-identification="selection-on-observables">
            <h5><a href="part2/meta_learner.ipynb">Meta-Learners</a></h5>
            <p>S/T/X-Learner를 활용한 CATE 추정</p>
            <div class="tags">
                <span class="tag">CATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="cate" data-data="cross-sectional" data-treatment="binary" data-identification="selection-on-observables">
            <h5><a href="part2/cate_with_nn.ipynb">CATE with Neural Networks</a></h5>
            <p>DragonNet, CEVAE 등 딥러닝 기반 CATE 추정</p>
            <div class="tags">
                <span class="tag">CATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">Neural Net</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="ate" data-data="cross-sectional" data-treatment="binary" data-identification="backdoor">
            <h5><a href="part2/backdoor_criterion.ipynb">Backdoor Criterion</a></h5>
            <p>인과 그래프 기반 백도어 조정으로 ATE 추정</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">Backdoor</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="" data-data="cross-sectional" data-treatment="" data-identification="">
            <h5><a href="part2/causal_discovery.ipynb">Causal Discovery</a></h5>
            <p>LiNGAM, PC, GES 등으로 인과 구조 학습</p>
            <div class="tags">
                <span class="tag">DAG</span>
                <span class="tag">횡단면</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="ate" data-data="longitudinal" data-treatment="time-varying" data-identification="selection-on-observables">
            <h5><a href="part2/MSM.ipynb">Marginal Structural Models</a></h5>
            <p>시변 처치에 대한 ATE 추정</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">종단</span>
                <span class="tag">시변</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="policy" data-data="cross-sectional" data-treatment="binary" data-identification="selection-on-observables">
            <h5><a href="part2/policy_learning.ipynb">Policy Learning</a></h5>
            <p>최적 정책 학습 및 평가</p>
            <div class="tags">
                <span class="tag">Policy</span>
                <span class="tag">횡단면</span>
                <span class="tag">이진</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="cate" data-data="cross-sectional" data-treatment="binary" data-identification="selection-on-observables">
            <h5><a href="part2/interpretability.ipynb">CATE Interpretability</a></h5>
            <p>Causal Tree, SHAP 등으로 CATE 해석</p>
            <div class="tags">
                <span class="tag">CATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">해석</span>
            </div>
        </div>

        <!-- Examples -->
        <div class="notebook-card" data-estimand="ate" data-data="cross-sectional" data-treatment="binary" data-identification="frontdoor">
            <h5><a href="examples/frontdoor_criterion.ipynb">Frontdoor Criterion</a></h5>
            <p>매개 변수를 통한 인과 효과 식별</p>
            <div class="tags">
                <span class="tag">ATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">Frontdoor</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="policy" data-data="cross-sectional" data-treatment="binary" data-identification="selection-on-observables">
            <h5><a href="examples/budget_constrained_optimization.ipynb">Budget Optimization</a></h5>
            <p>비용 제약 하에서 최적 처치 할당</p>
            <div class="tags">
                <span class="tag">Policy</span>
                <span class="tag">횡단면</span>
                <span class="tag">최적화</span>
            </div>
        </div>

        <div class="notebook-card" data-estimand="cate" data-data="cross-sectional" data-treatment="binary" data-identification="selection-on-observables">
            <h5><a href="examples/fractional_uplift.ipynb">Fractional Uplift</a></h5>
            <p>처치 효과 이질성 기반 타겟팅</p>
            <div class="tags">
                <span class="tag">CATE</span>
                <span class="tag">횡단면</span>
                <span class="tag">Uplift</span>
            </div>
        </div>
    </div>
</div>

<script>
let activeFilters = {};

function toggleFilter(el) {
    const category = el.dataset.category;
    const value = el.dataset.value;

    el.classList.toggle('active');

    if (!activeFilters[category]) {
        activeFilters[category] = new Set();
    }

    if (el.classList.contains('active')) {
        activeFilters[category].add(value);
    } else {
        activeFilters[category].delete(value);
    }

    applyFilters();
}

function applyFilters() {
    const cards = document.querySelectorAll('.notebook-card');
    let visibleCount = 0;

    cards.forEach(card => {
        let show = true;

        for (const [category, values] of Object.entries(activeFilters)) {
            if (values.size === 0) continue;

            const cardValues = (card.dataset[category] || '').split(',');
            const hasMatch = [...values].some(v => cardValues.includes(v));

            if (!hasMatch) {
                show = false;
                break;
            }
        }

        if (show) {
            card.classList.remove('hidden');
            visibleCount++;
        } else {
            card.classList.add('hidden');
        }
    });

    document.getElementById('result-count').textContent =
        visibleCount === 19 ? '전체 19개 노트북' : `${visibleCount}개 노트북 일치`;
}

function resetFilters() {
    activeFilters = {};
    document.querySelectorAll('.filter-tag').forEach(el => el.classList.remove('active'));
    document.querySelectorAll('.notebook-card').forEach(el => el.classList.remove('hidden'));
    document.getElementById('result-count').textContent = '전체 19개 노트북';
}
</script>
```
