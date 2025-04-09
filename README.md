# Doyle-Fuller-Newman (DFN) 모델을 이용한 전극 입자 표면농도 계산 문제

본 문제는 리튬이온 배터리의 전극 입자 내부에서 리튬의 확산현상을 기술하는 **DFN (Doyle-Fuller-Newman)** 모델을 기반으로 합니다. 아래의 PDE와 경계 조건을 만족하도록 수치해석 방법을 사용하여, 전극 입자 표면의 리튬 농도 \(c_{s,\text{surf}}(t)\)를 계산하는 것이 목표입니다.

## 1. 문제 정의 (Problem Statement)

전극 입자 내의 리튬 농도 $\( c_s(r,t) \)$는 다음 PDE로 표현됩니다. 구좌표계의 1차원 구형 대칭(spherical symmetry) 확산 방정식입니다.

$\[
\frac{\partial c_s}{\partial t} = \frac{D_s}{r^2}\frac{\partial}{\partial r}\left(r^2\frac{\partial c_s}{\partial r}\right),\quad 0< r < R_s,\quad t>0
\]$

여기서,

- $\( c_s(r,t) \)$: 입자 내 리튬 농도 (mol/m³)
- $\( D_s \)$: 고체상 확산계수 (m²/s)
- $\( r \)$: 반지름 방향 좌표 (m)
- $\( R_s \)$: 입자 반경 (m)

## 2. 초기조건 (Initial Condition)

전극 입자는 완충 상태(초기 State-of-Charge, SoC 100%)로 균일한 농도를 갖습니다.

$\[
c_s(r,0) = c_{s,\text{init}},\quad 0\le r\le R_s
\]$

초기 농도는 최대 농도의 특정 비율로 주어질 수 있습니다.

- 예시: $\( c_{s,\text{init}} = \theta_{\text{init}}\cdot c_{s,\text{max}} \)$,  
$(\(\theta_{\text{init}}\)$: 초기 SoC 비율, $\(0 \leq \theta_{\text{init}} \leq 1\))$

## 3. 경계조건 (Boundary Conditions)

| 위치 | 종류 | 수학적 표현 |
|---|---|---|
| 입자 중심 $(\( r=0 \))$ | 대칭조건 | $\(\frac{\partial c_s}{\partial r}\bigg|_{r=0}=0\)$ |
| 입자 표면 $(\( r=R_s \))$ | 전기화학 flux 조건 | $\(-D_s\frac{\partial c_s}{\partial r}\bigg|_{r=R_s}=\frac{j}{a_sF}\)$ |

- $\( j \)$: 계면 반응 전류 밀도 (A/m³)
- $\( a_s = \frac{3\varepsilon_s}{R_s} \)$: 전극 입자의 비표면적 (1/m)
- $\( F \)$: Faraday 상수 (96485 C/mol)

## 4. 계면 반응 전류 밀도 (Interfacial Reaction Current Density)

계면 반응 전류 밀도 $\(j\)$는 Butler-Volmer 식에 따라 결정됩니다.

$\[
j = j_0\left[\exp\left(\frac{\alpha_aF\eta}{RT}\right)-\exp\left(-\frac{\alpha_cF\eta}{RT}\right)\right]
\]$

- $\( j_0 \)$: 교환 전류 밀도 (A/m³)
- $\( \eta = \phi_s - \phi_e - U(c_{s,\text{surf}}) \)$: 과전압 (V)
- $\(\alpha_a, \alpha_c\)#: 전하 전달 계수 (보통 0.5)
- $\(R\)$: 기체 상수 (8.314 J/mol·K)
- $\(T\)$: 절대 온도 (K)
- $\(U(c_{s,\text{surf}})\)$: 입자 표면 농도에 의존하는 개방회로전압(OCV)

여기서, $\(j_0\)$은 농도의 함수로 다음 형태를 갖습니다.

$\[
j_0=k\cdot c_e^{0.5}\cdot c_{s,\text{surf}}^{0.5}\cdot(c_{s,\text{max}}-c_{s,\text{surf}})^{0.5}
\]$

$(\(k\)$: 반응 속도 상수, $\(c_e\)$: 전해질 농도)

## 5. 주요 파라미터 값 (Example Parameters)

| 파라미터 | 기호 | 단위 | 예시 값 |
|---|---|---|---|
| 고체 확산계수 | $\(D_s\)$ | m²/s | $\(3.9\times10^{-14}\)$ |
| 입자 반경 | $\(R_s\)$ | m | $\(1\times10^{-5}\)$ |
| 최대 농도 | $\(c_{s,\text{max}}\)$ | mol/m³ | $\(24983\)$ (음극), $\(51218\)$ (양극) |
| 활성물질 부피분율 | $\(\varepsilon_s\)$ | - | $\(0.6\)$ (음극), $\(0.5\)$ (양극) |
| Faraday 상수 | $\(F\)$ | C/mol | $\(96485\)$ |

## 6. 표면 농도 계산 및 결과 표현

최종 목표는 PDE를 수치적으로 풀어 **입자 표면 농도 $\(c_{s,\text{surf}}\)$** 를 다음과 같이 구하는 것입니다.

$\[
c_{s,\text{surf}}(t)=c_s(R_s,t)
\]$

다음 시간 범위와 조건에서 계산하시오.

- 시간 범위: $\(0 \le t \le 3600\)$ s (1시간, 방전 프로파일)
- 인가 전류 조건: 상수 C-rate 방전조건 (예: 0.5C 방전 전류 등)

---

## 📌 **평가 기준**

다음 항목을 포함하여 문제를 풀이할 것:

- PDE와 경계조건의 명확한 정의 및 기술 (20점)
- 적절한 수치해석 방법의 선택과 구현 전략 (20점)
- 사용된 파라미터의 설명과 단위 명시 (10점)
- 초기조건 및 경계조건 적용의 정확성 (20점)
- 계산 결과 (입자 표면 농도)의 적절한 시각화와 해석 (20점)
- 결과의 타당성 및 물리적 의미에 대한 고찰 (10점)

---

이 문제는 리튬이온 배터리의 물리적 모델링 및 수치해석적 풀이 역량을 평가하기 위한 목적으로 작성되었습니다. 명확한 수치해석 프로세스와 결과 해석 능력을 중점적으로 평가합니다.

제공된 정보를 참고하여 수치해석 솔루션을 구현하고, 결과를 효과적으로 시각화 및 분석하십시오.
