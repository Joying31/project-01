# 📊 Macro-Economic Regime Switching Model using Hybrid HMM

> 거시경제 및 금융 데이터를 활용한 HMM(Hidden Markov Model) 기반 시장 국면 분류 및 최적화 프로젝트

## 📝 Project Overview (프로젝트 요약)

본 프로젝트는 다차원 금융/거시경제 시계열 데이터를 활용하여 금융 시장의 구조적 변화(Regime Shift)를 포착하는 머신러닝 파이프라인을 구축합니다.

단순한 알고리즘 적용을 넘어, **순수 통계적 차원 축소(PCA)가 가지는 '경제적 해석력 상실(2008년 금융위기 포착 실패)'의 문제를 규명하고, 핵심 금융 도메인 지식(시장 수익률)을 결합한 하이브리드 접근법을 통해 통계적 안정성과 직관적 해석력을 모두 갖춘 최적의 모델**을 도출했습니다.

## 💡 Key Insights & Dilemma Solved

* The Black-box Dilemma: PCA를 통해 데이터의 분산 설명력(Out-of-Sample 성능)을 기계적으로 극대화했을 때, 오히려 2008년 글로벌 금융위기와 같은 꼬리 위험(Tail Risk) 시그널이 노이즈와 섞여 희석되는(Blurring) 현상을 발견했습니다.
* Domain Anchoring: 위 문제를 해결하기 위해 거시경제를 요약하는 PCA 주성분에 시장의 방향성을 나타내는 '수익률(Return)' 변수를 명시적으로 추가(Anchoring)했습니다. 그 결과, 2008년 위기 포착률이 40%에서 100%로 수직 상승하며 모델의 경제적 설명력이 극적으로 개선되었습니다.

---

## 🚀 Project Workflow (진행 단계)

### Phase 1: Foundation & Exploration (`01`~`02`)
* 기존 연구(3-state GMM-HMM, 9개 거시 변수) 코드를 모듈화하여 베이스라인을 구축했습니다.
* BIC/AIC 기준 Grid Search를 통해 훈련(-2019) 및 검증(2020-) 데이터에 적합한 HMM의 State 수와 Mixture 수의 탐색 범위를 확립했습니다.

### Phase 2: Feature Engineering & Global Optimization (`03`~`04`)
* 다차원 데이터의 노이즈 제거를 위해 변수 축소(PCA vs Domain) 방식을 1차 비교했습니다.
* `PCA 차원 수` $\times$ `n_components` $\times$ `n_mix` 총 40개 조합에 대한 전역 최적화를 수행하여 [PCA 3차원 + 4-State GaussianHMM]을 통계적 최적 모델로 도출했습니다.

### Phase 3: The Interpretability Crisis (`05`)
* 문제 발견: Phase 2의 통계적 최적 모델에 대한 경제적 검증을 수행한 결과, 통계적 지표(KS Test 등)는 우수했으나 2008년 금융위기를 40%밖에 포착하지 못하는 치명적 한계를 발견했습니다.
* "숫자는 좋으나 스토리가 없는 모델"이 가진 실무적 한계를 확인했습니다.

### Phase 4: Resolution & Final Model (`06`)
* 순수 도메인 기반, 순수 PCA, 그리고 **하이브리드(PCA + 도메인)** 모델군을 전면 비교 평가했습니다.
* 최종 선정 (Model B2): `PCA 3차원(거시경제 요약) + log_return(시장 방향성 앵커)`
* 최종 성과:
  * 2008년 금융위기 '100% 포착', 2020년 팬데믹 73% 포착
  * KS Test 100% 통과, 국면 지속기간 397일의 안정성 확보
  * 노이즈성 전환 확률 최소화 (0.0025)

---

## 📁 Repository Structure

```text
.
├── notebooks/
│   ├── 01_baseline_reproduction.ipynb  # 베이스라인 재현
│   ├── 02_optimal_n_states.ipynb       # HMM 구조 파라미터 탐색
│   ├── 03_variable_selection.ipynb     # 변수 축소 방식 기초 비교
│   ├── 04_joint_optimization.ipynb     # Grid Search를 통한 전역 최적화
│   ├── 05_regime_validation.ipynb      # 통계적 최적 모델의 경제적 검증 (한계 발견)
│   └── 06_domain_vs_pca.ipynb          # 하이브리드 모델 실험 및 최종 결론
├── src/                                # 재사용 가능한 모듈화된 파이썬 스크립트
│   ├── data_loader.py
│   ├── features.py
│   ├── hmm_model.py
│   └── utils.py
└── results/                            # 모델 평가 지표, 히트맵, 스코어카드 등
