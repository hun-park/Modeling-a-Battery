# Doyle-Fuller-Newman (DFN) 모델 PDE 문제

다음 PDE는 리튬이온 배터리의 전극 입자 내부에서 리튬의 확산 현상을 설명하는 Doyle-Fuller-Newman (DFN) 모델의 일부분이다. 이를 수치적으로 해결하여 입자 표면의 리튬 농도를 예측하는 문제이다.

---

## 1. PDE의 수학적 표현

다음과 같은 구좌표계에서의 구형 입자 내 리튬 확산 방정식을 고려한다.

\[\frac{\partial c_s}{\partial t} = \frac{D_s}{r^2}\frac{\partial}{\partial r}\left(r^2\frac{\partial c_s}{\partial r}\right),\quad 0 \leq r \leq R_s, \quad t>0\]

- \( c_s(r,t) \): 입자 내부의 리튬 농도 \((\text{mol/m}^3)\)
- \( D_s \): 확산계수 \((\text{m}^2/\text{s})\)
- \( r \): 입자 내 반경 방향 좌표 \((\text{m})\)
- \( R_s \): 입자의 반지름 \((\text{m})\)

---

## 2. 초기조건 (Initial Condition)

초기 농도는 입자 내부에 균일하다고 가정한다:

\[c_s(r,0)=c_{s,\text{init}},\quad 0\le r\le R_s\]

여기서 \( c_{s,\text{init}} = \theta_{\text{init}} \cdot c_{s,\text{max}} \)이며, \(\theta_{\text{init}}\)는 초기 충전 상태(SoC)를 나타낸다.

---

## 3. 경계조건 (Boundary Conditions)

다음 경계조건을 만족한다:

| 위치             | 조건                 | 수학적 표현                                                |
|----------------|----------------------|------------------------------------------------------|
| 입자 중심 \( r=0 \)  | 대칭 조건(Flux=0)       | \(\frac{\partial c_s}{\partial r}\big|_{r=0}=0\) |
| 입자 표면 \( r=R_s \) | 전기화학 반응 Flux 조건 | \(-D_s\frac{\partial c_s}{\partial r}\big|_{r=R_s}=\frac{j}{a_sF}\) |

- \( j \): 계면 반응 전류밀도 \((\text{A/m}^3)\)
- \( a_s=\frac{3\varepsilon_s}{R_s} \): 비표면적 \((\text{m}^{-1})\)
- \( \varepsilon_s \): 활성물질 부피분율
- \( F \): Faraday 상수 (96485 C/mol)

---

## 4. 파라미터 정의

| 기호                | 의미                        | 단위             |
|-------------------|---------------------------|----------------|
| \( D_s \)         | 고체상 확산 계수               | \(\text{m}^2/\text{s}\) |
| \( R_s \)         | 입자 반지름                   | \(\text{m}\)         |
| \( c_{s,\text{max}} \) | 최대 리튬 농도                 | \(\text{mol/m}^3\)    |
| \( \varepsilon_s \) | 활성물질 부피분율               | 무차원              |
| \( j \)           | 계면 전류 밀도                 | \(\text{A/m}^3\)     |
| \( a_s \)         | 입자 비표면적                  | \(\text{m}^{-1}\)    |
| \( F \)           | Faraday 상수                | \(\text{C/mol}\)     |

---

## 5. 문제 (Problem Statement)

위의 PDE와 주어진 초기조건, 경계조건을 만족하도록 수치해석 기법을 활용하여 다음 값을 계산하라:

- **입자 표면 농도 \( c_{s,\text{surf}}(t)=c_s(R_s,t) \)** 를 시간에 따라 계산하고 그래프로 나타내시오.

---

## 6. 참고 사항

- PDE의 공간방향 이산화 방법으로는 유한차분법(FDM) 또는 유한요소법(FEM)을 사용할 수 있다.
- 시간방향 이산화는 Euler 또는 Crank-Nicolson 방법 등을 고려할 수 있다.
- 계면 반응 전류 밀도 \( j \)는 Butler-Volmer 방정식 등을 이용하여 계산될 수 있으며, 별도의 입력 조건으로 주어질 수 있다.

---

**제출물:**

- PDE 문제의 수치해석적 접근 방법을 서술하시오.
- 수치적 결과(입자 표면 농도 \( c_{s,\text{surf}}(t) \))를 그래프로 제시하시오.

