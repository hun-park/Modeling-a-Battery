# 배터리 모델링

---

**문제 1. (총 30점)**

리튬이온 배터리의 전기화학적 거동을 상세히 예측하는 데 널리 사용되는 Doyle-Fuller-Newman (DFN) 모델은 다공성 전극 이론(porous electrode theory)과 농축 용액 이론(concentrated solution theory)을 기반으로 합니다. 이 모델은 전극 두께 방향(x축)과 활물질 입자 내부의 반경 방향(r축) 모두에서 일어나는 복잡한 연동(coupled) 전달 현상 및 반응을 고려합니다.

DFN 모델의 프레임워크 내에서, 전극 내 특정 위치 $x$와 시간 $t$에서의 활물질 **입자 표면 농도 $c_{s,surf}(x, t)$** 를 결정하는 것은 모델의 정확성과 성능 예측에 매우 중요합니다.

**(a) 입자 내부 확산 (Particle-Scale Diffusion) (10점)**

전극 내 위치 $x$에 존재하는 단일 구형(spherical) 활물질 입자 ($0 \le r \le R_p$) 내부에서의 리튬 이온 농도 $c_s(x, r, t)$ 변화를 기술하는 편미분 방정식(PDE)과 해당 경계 조건(BCs) 및 초기 조건(IC)을 명시하시오. 특히, 입자 표면($r=R_p$)에서의 경계 조건이 국소적인 기공 벽 플럭스 밀도 $j_n(x, t)$ [mol m⁻² s⁻¹]와 어떻게 연관되는지 설명하시오.

**(b) 계면 반응 속도 및 연동 (Interfacial Kinetics and Coupling) (10점)**

기공 벽 플럭스 밀도 $j_n(x, t)$는 입자 내부 확산 문제와 전극 두께 방향(x축)의 거동을 연결하는 핵심적인 항입니다. $j_n(x, t)$가 일반적으로 사용되는 Butler-Volmer 식을 통해 어떻게 표현되는지 설명하고, 이 식이 국소적인 고체상 전위($\phi_s(x,t)$), 전해액상 전위($\phi_e(x,t)$), 전해액 농도($c_e(x,t)$), 그리고 구하고자 하는 **표면 농도 $c_{s,surf}(x,t)$** 와 어떤 관계를 갖는지 논하시오. DFN 모델에서 이 항이 가지는 물리적 의미와 연동(coupling)에서의 중요성을 강조하시오.

**(c) 전체 DFN 모델 및 해법 접근 (Full DFN Model and Solution Approach) (10점)**

$j_n(x, t)$를 정확히 계산하는 데 필요한 국소 변수들($\phi_s(x,t)$, $\phi_e(x,t)$, $c_e(x,t)$)은 전극 두께 방향(x축)에서의 전하 보존 및 물질 보존 법칙을 따르는 별도의 연동된 PDE 시스템에 의해 결정됩니다. 이 전극 수준(electrode-scale)의 방정식 시스템을 간략히 언급하고 (모든 방정식을 상세히 적을 필요는 없음), 입자 수준(particle-scale)의 확산 방정식과 어떻게 연동되는지 설명하시오. 최종적으로, DFN 모델 전체를 풀어 특정 위치와 시간에서의 **입자 표면 농도 $c_{s,surf}(x, t)$** 를 얻기 위해 필요한 전반적인 **해법 접근 방식(solution methodology)** 에 대해 논하시오 (예: 필요한 수치 해석 기법). 모델을 푸는 데 필요한 주요 파라미터들을 간략히 언급하시오.

---

# 모범 답안

---

**문제 1. DFN 모델 기반 입자 표면 농도 계산**

**(a) 입자 내부 확산 (Particle-Scale Diffusion) (10점)**

전극 내 특정 위치 $x$에 존재하는 구형 활물질 입자 내부에서의 리튬 이온 농도 $c_s(x, r, t)$는 Fick의 제2법칙에 의해 지배됩니다. 구형 좌표계에서 이 법칙은 다음과 같은 편미분 방정식(PDE)으로 표현됩니다:

$$
\frac{\partial c_s(x, r, t)}{\partial t} = \frac{1}{r^2} \frac{\partial}{\partial r} \left( D_s r^2 \frac{\partial c_s(x, r, t)}{\partial r} \right) \quad (0 < r < R_p)
$$

여기서 $D_s$는 고체상 확산 계수 [m²/s], $R_p$는 입자 반경 [m]입니다.

이 PDE를 풀기 위한 경계 조건(BCs)과 초기 조건(IC)은 다음과 같습니다:

1.  **중심에서의 경계 조건 (BC at r=0):** 입자 중심에서는 농도 구배가 0이어야 하므로 (대칭성):
    $$
    \left. \frac{\partial c_s(x, r, t)}{\partial r} \right|_{r=0} = 0
    $$

2.  **표면에서의 경계 조건 (BC at r=R_p):** 입자 표면을 통한 리튬 이온의 확산 플럭스는 Fick의 제1법칙에 의해 주어지며, 이는 계면에서의 전기화학 반응에 의한 국소적인 기공 벽 플럭스 밀도 $j_n(x, t)$ [mol m⁻² s⁻¹]와 같습니다. 부호는 입자 밖으로 나가는 플럭스를 양수로 정의할 때 다음과 같습니다:
    $$
    -D_s \left. \frac{\partial c_s(x, r, t)}{\partial r} \right|_{r=R_p} = j_n(x, t)
    $$
    즉, 입자 표면에서의 농도 구배는 해당 위치 $x$에서의 순수 반응 속도($j_n$)에 의해 결정됩니다.

3.  **초기 조건 (IC at t=0):** 일반적으로 초기 상태에서는 입자 내부 농도가 균일하다고 가정합니다:
    $$
    c_s(x, r, t=0) = c_{s,0}(x)
    $$
    여기서 $c_{s,0}(x)$는 초기 농도로, 전극 전체에 걸쳐 균일할 수도 있고 위치 $x$에 따라 다를 수도 있습니다.

**(b) 계면 반응 속도 및 연동 (Interfacial Kinetics and Coupling) (10점)**

기공 벽 플럭스 밀도 $j_n(x, t)$는 활물질 입자 표면에서 일어나는 리튬 이온의 삽입/탈리(intercalation/deintercalation) 반응 속도를 나타냅니다. 이는 일반적으로 Butler-Volmer 식을 사용하여 모델링됩니다:

$$
j_n(x, t) = \frac{i_0(x, t)}{F} \left[ \exp\left(\frac{\alpha_a F}{RT} \eta(x, t)\right) - \exp\left(-\frac{\alpha_c F}{RT} \eta(x, t)\right) \right]
$$

여기서 각 항의 의미는 다음과 같습니다:
* $i_0(x, t)$: 교환 전류 밀도 [A/m²]. 이는 반응 평형 상태에서의 반응 속도를 나타내며, 일반적으로 농도에 의존합니다: $i_0 = k (c_e)^{\alpha_a} (c_{s,max} - c_{s,surf})^{\alpha_a} (c_{s,surf})^{\alpha_c}$. ($k$: 반응 속도 상수, $c_{s,max}$: 최대 고체 농도)
* $F$: 패러데이 상수 [C/mol]
* $R$: 기체 상수 [J/(mol·K)]
* $T$: 절대 온도 [K]
* $\alpha_a, \alpha_c$: 양극(산화) 및 음극(환원) 전달 계수 (보통 $\alpha_a + \alpha_c = 1$)
* $\eta(x, t)$: 계면 과전압 [V]. 이는 반응을 구동하는 전기화학적 원동력으로, 다음과 같이 정의됩니다:
    $$
    \eta(x, t) = \phi_s(x, t) - \phi_e(x, t) - U(c_{s,surf}(x, t))
    $$
    * $\phi_s(x, t)$: 국소적인 고체상 전위 [V]
    * $\phi_e(x, t)$: 국소적인 전해액상 전위 [V]
    * $U(c_{s,surf}(x, t))$: 국소적인 고체 표면 농도 $c_{s,surf}(x, t)$에 해당하는 평형 전위 (Open Circuit Potential, OCP) [V]

**물리적 의미 및 연동 중요성:**
$j_n(x, t)$는 국소적인 전기화학적 구동력($\eta$)에 의해 결정되는 순수 반응 속도(단위 면적당 몰 플럭스)입니다. 이 항은 DFN 모델의 핵심적인 **연동(coupling) 항**입니다.
1.  **입자 내부 확산 문제(a)에서는:** $j_n$이 표면($r=R_p$)에서의 경계 조건(플럭스)을 결정합니다.
2.  **전극 두께 방향 문제(c)에서는:** $j_n$이 전해액 농도($c_e$), 전해액 전위($\phi_e$), 고체상 전위($\phi_s$) 방정식에서 소스/싱크(source/sink) 항으로 작용합니다 (아래 (c) 참조).

따라서 $j_n$은 입자 내부의 상태($c_{s,surf}$를 통해 $U$와 $i_0$에 영향)와 전극 두께 방향의 전해액 및 고체상의 상태($c_e, \phi_e, \phi_s$를 통해 $\eta$와 $i_0$에 영향)를 연결하는 다리 역할을 합니다.

**(c) 전체 DFN 모델 및 해법 접근 (Full DFN Model and Solution Approach) (10점)**

$j_n(x, t)$를 계산하는 데 필요한 국소 변수들($\phi_s(x,t)$, $\phi_e(x,t)$, $c_e(x,t)$)은 전극 두께 방향(x축)에서의 전하 보존 및 물질 보존 법칙에 의해 결정됩니다. 주요 방정식들은 다음과 같습니다 (1D 가정):

* **고체상 전하 보존 (Ohm's Law):**
    $$
    \frac{\partial}{\partial x} \left( \sigma_{eff} \frac{\partial \phi_s}{\partial x} \right) = a_s F j_n
    $$
    ($\sigma_{eff}$: 유효 고체 전도도, $a_s$: 단위 부피당 비표면적)

* **전해액상 전하 보존 (Modified Ohm's Law):**
    $$
    \frac{\partial}{\partial x} \left( \kappa_{eff} \frac{\partial \phi_e}{\partial x} \right) + \frac{\partial}{\partial x} \left( \kappa_{D,eff} \frac{\partial \ln c_e}{\partial x} \right) = -a_s F j_n
    $$
    ($\kappa_{eff}$: 유효 이온 전도도, $\kappa_{D,eff}$: 확산 전도도 항)

* **전해액상 물질 보존 (Concentrated Solution Theory):**
    $$
    \epsilon_e \frac{\partial c_e}{\partial t} = \frac{\partial}{\partial x} \left( D_{e,eff} \frac{\partial c_e}{\partial x} \right) + (1-t_+^0) a_s j_n
    $$
    ($\epsilon_e$: 전해액 부피 분율, $D_{e,eff}$: 유효 확산 계수, $t_+^0$: 이동수(transference number))

**연동 방식:**
위의 전극 수준 방정식들은 $j_n(x, t)$를 소스/싱크 항으로 포함하며, 이 방정식들을 풀면 각 위치 $x$에서의 $\phi_s, \phi_e, c_e$를 얻을 수 있습니다. 이 값들은 (b)의 Butler-Volmer 식에 대입되어 $j_n(x, t)$를 계산하는 데 사용됩니다. 계산된 $j_n(x, t)$는 다시 (a)의 입자 내부 확산 방정식의 표면 경계 조건으로 사용되어 입자 내부 농도 프로파일 $c_s(x, r, t)$ 및 표면 농도 $c_{s,surf}(x, t)$를 업데이트합니다. 업데이트된 $c_{s,surf}(x, t)$는 다시 Butler-Volmer 식의 $U$와 $i_0$에 영향을 미치므로, 전체 시스템은 강하게 연동된(strongly coupled) 비선형(non-linear) 시스템입니다.

**해법 접근 방식 (Solution Methodology):**
DFN 모델은 시간($t$)과 두 개의 공간 차원($x$, $r$)에 대한 연동된 비선형 편미분 방정식 시스템(Pseudo-2D)이므로 해석적 해를 구하기 어렵습니다. 따라서 **수치 해석적 방법**이 필수적입니다.
1.  **공간 이산화 (Spatial Discretization):** 전극 두께 방향(x축)과 입자 반경 방향(r축)에 대해 유한 차분법(FDM), 유한 부피법(FVM), 또는 유한 요소법(FEM) 등을 사용하여 PDE를 대수 방정식 시스템으로 변환합니다.
2.  **시간 이산화 (Temporal Discretization):** 시간에 대해 이산화합니다. 확산 방정식과 반응 속도 항으로 인해 시스템이 종종 강성(stiff)하므로, 안정성을 위해 암시적(implicit) 시간 전진 기법 (예: Backward Euler, Crank-Nicolson)이 선호됩니다.
3.  **비선형성 처리 (Handling Non-linearity):** Butler-Volmer 식, 농도 의존적 파라미터($D_s, D_e, \kappa, i_0, U$ 등) 때문에 시스템이 비선형입니다. 각 시간 스텝에서 비선형 대수 방정식 시스템을 풀기 위해 Newton-Raphson과 같은 반복적(iterative) 해법이 필요합니다.
4.  **연동 해법 (Coupled Solution):** 모든 변수($c_s, c_e, \phi_s, \phi_e$)를 동시에 푸는 완전 연동(fully coupled) 방식 또는 변수들을 순차적으로 푸는 분리(segregated) 방식을 사용할 수 있습니다.

**주요 파라미터:**
모델을 풀기 위해서는 다양한 물리화학적 파라미터가 필요합니다. 주요 파라미터는 다음과 같습니다:
* **기하학적:** 전극/분리막 두께, 입자 반경($R_p$), 고체/전해액 부피 분율($\epsilon_s, \epsilon_e$), 비표면적($a_s$).
* **전달 물성:** 고체 확산 계수($D_s$), 전해액 확산 계수($D_e$), 고체 전도도($\sigma$), 전해액 전도도($\kappa$), 이동수($t_+^0$). (이들 중 다수는 농도/온도 의존성을 가질 수 있음)
* **반응 속도론:** 반응 속도 상수($k$) 또는 기준 교환 전류 밀도($i_{0,ref}$), 전달 계수($\alpha_a, \alpha_c$).
* **열역학:** 평형 전위($U(c_s)$), 활동도 계수 관련 항(Thermodynamic factor).
* **초기/경계 조건:** 초기 농도($c_{s,0}, c_{e,0}$), 인가 전류/전압.

이러한 수치 해석적 접근을 통해 시간에 따른 각 위치 $x$에서의 $c_s(x, r, t)$ 분포를 계산할 수 있으며, 특히 $r=R_p$에서의 값인 **$c_{s,surf}(x, t)$** 를 얻을 수 있습니다.

---
