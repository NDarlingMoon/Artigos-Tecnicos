# Testes e Mocks
Quando menor, já cheguei a me imaginar casando com pessoas do mesmo gênero... mas nunca realmente considerei a ideia. Até meados de meus 21 anos...  
Fui a um bar com minha "amiga" e, num momento em que o álcool tomava conta de nossas mentes, ela começou a flertar comigo. Fiquei me fazendo de idiota, querendo prestar atenção na TV (Que, aliás, estava passando "Konosuba"), mas, acabei cedendo e ela me meteu um beijão.  
Naquela noite, quase nos expulsaram com medo que fôssemos transformar o sofá do bar em motel. Passado esse dia, namoramos por quase 1 ano e isso consolidou minha fama de *Sáfica Cafajeste*.  
O que essa história nos ensina? Que precisamos testar as coisas! A imaginação é um bom campo experimental, mas ela não supre a realidade. Ao escrever um código, ele funciona? Você pode dizer que sim, mas, e os possíveis erros que vão vir? Você os testou ou você apenas **acha** que vai funcionar? Testes Unitários existem em todas as linguagens orientadas a objetos e possuem justamente a função de simular diferentes cenários possíveis em seu código, para garantir que tudo esteja saindo como planejado.
<p align="center"> <img src="../../../Images/MagiRevoKiss.png" width="100%"> <br> <em>Se está em dúvida sobre sua Orientação Sexual, meu maior conselho é "Caia de boca".</em> </p>

## Pytest
O Python vem com uma biblioteca para testes padrão, o `unittest`. Finja que ele não existe.

> Mas eu quero usar ele!

Não quer. Cale-se!  
`unittest` é antigo, engessado, verboso e tudo de mais podre que existe nesse mundo. Se quiser, use `unittest` para testar sua sanidade, mas aqui, utilizaremos `pytest`, que é extremamente mais simples, legível e poderoso.  
Comece instalando ele:

```bash
pip install pytest
```

E para rodar seus testes, basta digitar no terminal:

```bash
pytest
```

Não irá aparecer nada, afinal, nenhum teste foi criado ainda, oras. Mas veja como `pytest` é simples!

Pois bem, para criar testes, precisamos de métodos para serem testados. Vamos utilizar o método criado para fins didáticos a seguir como exemplo e criar testes em cima dele:

```python
import cmath
import math

# ============================================
# 1. REPRESENTAÇÃO DE NÚMEROS COMPLEXOS
# ============================================

print("=== REPRESENTAÇÃO DE NÚMEROS COMPLEXOS ===")

# Tensão e corrente como números complexos (forma retangular)
# V = a + jb, onde j = √(-1)
tensao = 100 + 50j  # 100V (parte real) + j50V (parte imaginária)
corrente = 5 - 2j   # 5A (parte real) - j2A (parte imaginária)

print(f"Tensão: {tensao} V")
print(f"Corrente: {corrente} A")

# Acessando partes real e imaginária
print(f"\nParte real da tensão: {tensao.real} V")
print(f"Parte imaginária da tensão: {tensao.imag} V")

# ============================================
# 2. OPERAÇÕES BÁSICAS
# ============================================

print("\n=== OPERAÇÕES BÁSICAS ===")

# Adição
tensao_total = tensao + (20 + 10j)
print(f"Tensão total (soma): {tensao_total} V")

# Multiplicação
impedancia = tensao / corrente
print(f"Impedância (Z = V/I): {impedancia} Ω")

# Conjugado complexo (importante para potência)
corrente_conjugada = corrente.conjugate()
print(f"Corrente conjugada: {corrente_conjugada} A")

# ============================================
# 3. FORMA POLAR E RETANGULAR
# ============================================

print("\n=== FORMA POLAR E RETANGULAR ===")

# Converter para forma polar (módulo e fase)
modulo_tensao = abs(tensao)
fase_tensao = cmath.phase(tensao)  # em radianos
fase_tensao_graus = math.degrees(fase_tensao)

print(f"Tensão em forma polar: {modulo_tensao:.2f} ∠ {fase_tensao_graus:.2f}°")

# Criar número complexo a partir de módulo e fase
modulo = 150  # V
fase_graus = 30  # graus
fase_rad = math.radians(fase_graus)
tensao_polar = cmath.rect(modulo, fase_rad)
print(f"150V ∠ 30° em forma retangular: {tensao_polar:.2f} V")

# ============================================
# 4. CÁLCULO DE POTÊNCIA EM CA
# ============================================

print("\n=== CÁLCULO DE POTÊNCIA EM CA ===")

# Potência complexa: S = V * I*
# onde I* é o conjugado complexo da corrente
potencia_complexa = tensao * corrente.conjugate()
print(f"Potência complexa S: {potencia_complexa:.2f} VA")

# Potência ativa (real) e reativa (imaginária)
potencia_ativa = potencia_complexa.real
potencia_reativa = potencia_complexa.imag

print(f"Potência ativa P: {potencia_ativa:.2f} W")
print(f"Potência reativa Q: {potencia_reativa:.2f} VAR")

# Fator de potência
fator_potencia = math.cos(cmath.phase(tensao) - cmath.phase(corrente))
print(f"Fator de potência: {fator_potencia:.3f}")

# ============================================
# 5. ANÁLISE DE CIRCUITO RLC SÉRIE
# ============================================

print("\n=== ANÁLISE DE CIRCUITO RLC SÉRIE ===")

# Parâmetros do circuito
frequencia = 60  # Hz
tensao_fonte = 120 + 0j  # 120V RMS, fase 0°
R = 10   # Ohms
L = 0.1  # Henries
C = 100e-6  # Farads

# Cálculo das reatâncias
omega = 2 * math.pi * frequencia  # frequência angular
XL = omega * L  # Reatância indutiva
XC = 1/(omega * C)  # Reatância capacitiva

# Impedâncias como números complexos
ZR = R + 0j      # Resistor: só parte real
ZL = 0 + XL*j    # Indutor: só parte imaginária positiva
ZC = 0 - XC*j    # Capacitor: só parte imaginária negativa

# Impedância total
Z_total = ZR + ZL + ZC
print(f"\nParâmetros do circuito:")
print(f"  Frequência: {frequencia} Hz")
print(f"  Resistência: {R} Ω")
print(f"  Reatância indutiva: {XL:.2f} Ω")
print(f"  Reatância capacitiva: {XC:.2f} Ω")
print(f"\nImpedância total: {Z_total:.2f} Ω")
print(f"Módulo da impedância: {abs(Z_total):.2f} Ω")
print(f"Fase da impedância: {math.degrees(cmath.phase(Z_total)):.2f}°")

# Corrente no circuito (Lei de Ohm)
corrente_circuito = tensao_fonte / Z_total
print(f"\nCorrente no circuito: {corrente_circuito:.2f} A")
print(f"Módulo da corrente: {abs(corrente_circuito):.2f} A RMS")
print(f"Fase da corrente: {math.degrees(cmath.phase(corrente_circuito)):.2f}°")

# Tensões em cada componente
VR = corrente_circuito * ZR
VL = corrente_circuito * ZL
VC = corrente_circuito * ZC

print(f"\nTensões nos componentes:")
print(f"  Tensão no resistor: {abs(VR):.2f} V, fase: {math.degrees(cmath.phase(VR)):.2f}°")
print(f"  Tensão no indutor: {abs(VL):.2f} V, fase: {math.degrees(cmath.phase(VL)):.2f}°")
print(f"  Tensão no capacitor: {abs(VC):.2f} V, fase: {math.degrees(cmath.phase(VC)):.2f}°")

# ============================================
# 6. FUNÇÃO PARA CÁLCULO DE POTÊNCIA
# ============================================

def calcular_potencia_ca(tensao, corrente):
    """
    Calcula parâmetros de potência em circuitos CA
    
    Args:
        tensao: número complexo representando a tensão
        corrente: número complexo representando a corrente
    
    Returns:
        Dicionário com todos os parâmetros calculados
    """
    # Potência complexa
    S = tensao * corrente.conjugate()
    
    # Potência ativa e reativa
    P = S.real
    Q = S.imag
    
    # Módulos
    V_rms = abs(tensao)
    I_rms = abs(corrente)
    
    # Fator de potência e ângulo
    fp = math.cos(cmath.phase(tensao) - cmath.phase(corrente))
    angulo_fp = math.degrees(math.acos(fp))
    
    return {
        'potencia_aparente': abs(S),
        'potencia_ativa': P,
        'potencia_reativa': Q,
        'fator_potencia': fp,
        'angulo_fator_potencia': angulo_fp,
        'tensao_rms': V_rms,
        'corrente_rms': I_rms
    }

print("\n=== FUNÇÃO DE CÁLCULO DE POTÊNCIA ===")
resultados = calcular_potencia_ca(tensao_fonte, corrente_circuito)

for chave, valor in resultados.items():
    print(f"{chave.replace('_', ' ').title()}: {valor:.2f}")

# ============================================
# 7. EXEMPLO PRÁTICO: MOTOR ELÉTRICO
# ============================================

print("\n=== EXEMPLO PRÁTICO: MOTOR ELÉTRICO ===")

# Dados do motor
tensao_motor = 220  # V RMS
corrente_motor = 10  # A RMS
fator_potencia_motor = 0.85  # atrasado (carga indutiva)
angulo_motor = -math.degrees(math.acos(fator_potencia_motor))

# Criar fasores (considerando tensão como referência com fase 0°)
V_motor = tensao_motor + 0j

# Corrente com ângulo de fase (negativo para atrasado)
I_motor = cmath.rect(corrente_motor, math.radians(angulo_motor))

print(f"Tensão do motor: {abs(V_motor):.1f} V ∠ {math.degrees(cmath.phase(V_motor)):.1f}°")
print(f"Corrente do motor: {abs(I_motor):.1f} A ∠ {math.degrees(cmath.phase(I_motor)):.1f}°")

# Calcular potência
resultados_motor = calcular_potencia_ca(V_motor, I_motor)

print(f"\nPotência aparente: {resultados_motor['potencia_aparente']:.1f} VA")
print(f"Potência ativa: {resultados_motor['potencia_ativa']:.1f} W")
print(f"Potência reativa: {resultados_motor['potencia_reativa']:.1f} VAR")
print(f"Fator de potência: {resultados_motor['fator_potencia']:.3f}")
```

> Números Complexos?  Não é um exemplo complexo demais?

Muitos guias e tutoriais por aí utilizam exemplos bestas de soma e fazem testes do tipo "2+2 é igual a 4?". Considero exemplos extremamente genéricos pois não demonstram o verdadeiro poder dos testes.  
Aqui, a escolha de um exemplo com cálculos de engenharia foi dada justamente por serem propensos a erros de arredondamentos e sinal, mostrando resultados inesperados que realmente podem ocorrer (E não testes idiotas do tipo "2+2 = 5?" para apitar que um teste falhou).  
Testes existem para capturar erros plausíveis. Exemplos simplistas não geram erros plausíveis.  
Se não se recorda ou nunca aprendeu números complexos, essa é uma oportunidade inefável para entendê-los. Liberte o Gênio que há em você!

Pois bem...  
Comece criando uma pasta chamada `tests` e um arquivo chamado `test_ac.py`. É **IMPRESCINDÍVEL** que o nome do arquivo comece com `test_`. Caso o nome seja outro, `pytest` irá ignorar completamente o arquivo na hora de testar os métodos.