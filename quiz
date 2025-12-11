import streamlit as st

# --- Configuração das Questões ---

# Definindo as questões, opções, respostas corretas, e pop-ups
QUESTOES = [
    {
        "id": 1,
        "pergunta": "Qual foi a música que eu mais ouvi pensando em você:",
        "opcoes": ["Aliança Tribalistas", "Se você quiser Chris", "Sol nos olhos Jorge e Mateus", "Fulminante Mumuzinho"],
        "correta": "Se você quiser Chris",
        "popup_acerto": (
            "Se você quiser\nA gente acorda antes do Sol nascer\nCom você, mulher\nEu decidi que eu quero viver"
        ),
        "observacao": (
            "Hoje - Jota Quest\n\"Hoje eu preciso te abraçar\nSentir teu cheiro de roupa limpa\nPra esquecer os meus anseios e dormir em paz\n\nHoje eu preciso ouvir qualquer palavra tua\nQualquer frase exagerada que me faça sentir alegria\nEm estar vivo\""
        )
    },
    {
        "id": 2,
        "pergunta": "Qual foi o dia em que nos vimos novamente após 6 meses?",
        "opcoes": ["02/08", "07/08", "05/08", "04/08"],
        "correta": "05/08",
        "popup_acerto": (
            "Dia 05, foi nessa terça-feira que descobrimos que faríamos aula juntos a noite inteira, sentamos juntos, conversamos pós aula. Se isso não foi meu sinal divino, eu não sei o que é."
        ),
        "observacao": ""
    },
    {
        "id": 3,
        "pergunta": "Qual foi o local em que eu saímos pela primeira vez juntos?",
        "opcoes": ["Parquinho Jardim das Américas", "Fábrica Gourmet", "Todeschini UFPR", "Ponto de ônibus Inter 2"],
        "correta": "Todeschini UFPR",
        "popup_acerto": (
            "Lembra aquela vez que eu perguntei se você jantou, nós compramos dois salgados, sentamos no ponto de ônibus e falamos sobre a vida?\nVocê me perguntou se eu tinha alguém, e eu só queria responder que eu queria que esse alguém fosse você"
        ),
        "observacao": "Te levar para faculdade não conta né?"
    }
]

# --- Configuração de Estado (Streamlit Sessions) ---
# Usamos st.session_state para armazenar dados entre as interações do usuário.

if 'status' not in st.session_state:
    st.session_state.status = 'inicio' # 'inicio', 'quiz', 'resultado'
if 'indice_questao' not in st.session_state:
    st.session_state.indice_questao = 0
if 'acertos' not in st.session_state:
    st.session_state.acertos = 0
if 'erros' not in st.session_state:
    st.session_state.erros = 0
if 'mostrar_popup' not in st.session_state:
    st.session_state.mostrar_popup = False
if 'popup_info' not in st.session_state:
    st.session_state.popup_info = {}

# --- Funções de Lógica ---

def iniciar_quiz():
    """Muda o estado para iniciar a primeira questão."""
    st.session_state.status = 'quiz'
    st.session_state.indice_questao = 0
    st.session_state.acertos = 0
    st.session_state.erros = 0
    st.session_state.mostrar_popup = False

def verificar_resposta(escolha):
    """Verifica a resposta e armazena as informações do popup."""
    
    questao_atual = QUESTOES[st.session_state.indice_questao]
    
    # 1. Armazena as informações do popup
    st.session_state.mostrar_popup = True
    
    if escolha == questao_atual["correta"]:
        st.session_state.acertos += 1
        st.session_state.popup_info = {
            "titulo": "✅ ACERTOU! (Resposta Correta)",
            "mensagem": questao_atual["popup_acerto"],
            "cor": "green",
            "observacao": questao_atual["observacao"]
        }
    else:
        st.session_state.erros += 1
        st.session_state.popup_info = {
            "titulo": f"❌ ERROU! (A correta era: {questao_atual['correta']})",
            "mensagem": "Poxa, não foi dessa vez. Tenta na próxima! 😉",
            "cor": "red",
            "observacao": ""
        }
    
    # 2. Re-renderiza para mostrar o popup e o botão de continuar
    st.experimental_rerun()


def avancar_questao():
    """Avança o índice ou termina o quiz."""
    st.session_state.indice_questao += 1
    st.session_state.mostrar_popup = False
    
    if st.session_state.indice_questao >= len(QUESTOES):
        st.session_state.status = 'resultado'
    
    # Re-renderiza para a próxima tela
    st.experimental_rerun()


# --- Funções de Renderização da Interface ---

def renderizar_tela_inicio():
    """Renderiza a tela inicial."""
    
    # Usando HTML e CSS simples (markdown) para estilizar
    st.markdown("""
        <style>
        .big-title {
            color: #ff69b4;
            text-align: center;
            font-size: 2.5em;
            padding: 20px;
            border: 3px solid #ff69b4;
            border-radius: 10px;
            margin-bottom: 30px;
        }
        </style>
    """, unsafe_allow_html=True)
    
    st.markdown("<div class='big-title'>Perguntas e Respostas para A MAIS LINDA</div>", unsafe_allow_html=True)
    
    # Botão de Entrada
    st.button("Entrar", on_click=iniciar_quiz, type="primary")


def renderizar_questao(questao):
    """Renderiza a pergunta e os botões de opção."""
    
    st.header(f"❓ Questão 0{questao['id']}")
    st.subheader(questao["pergunta"])
    
    # Cria os botões para as opções
    for opcao in questao["opcoes"]:
        # Usa uma lambda para passar o argumento 'opcao' para a função
        st.button(opcao, key=opcao, on_click=verificar_resposta, args=(opcao,), use_container_width=True)


def renderizar_popup():
    """Renderiza o popup de feedback e o botão de continuar."""
    
    info = st.session_state.popup_info
    
    # Determina a cor de fundo e borda para o popup
    popup_cor_borda = info["cor"]
    popup_cor_fundo = f"{popup_cor_borda}1A" # Cor clara com transparência
    
    # Estiliza o popup usando Markdown/HTML para uma aparência de destaque
    st.markdown(f"""
        <div style="border: 3px solid {popup_cor_borda}; padding: 15px; background-color: {popup_cor_fundo}; border-radius: 8px; margin-bottom: 20px;">
            <h4>{info["titulo"]}</h4>
            <p>{info["mensagem"].replace('\n', '<br>')}</p>
    """, unsafe_allow_html=True)

    # Adiciona observação, se houver
    if info.get("observacao"):
        st.markdown(f"""
            <p style='font-size: 0.9em; font-style: italic; border-top: 1px dashed #ccc; padding-top: 10px; margin-top: 10px;'>
                ***OBSERVAÇÃO:***<br>
                {info["observacao"].replace('\n', '<br>')}
            </p>
            </div>
        """, unsafe_allow_html=True)
    else:
        st.markdown("</div>", unsafe_allow_html=True)
        
    # Botão de Continuar/Finalizar
    is_ultima_questao = st.session_state.indice_questao == len(QUESTOES) - 1
    botao_texto = "Finalizar Quiz 💖" if is_ultima_questao else "Próxima Pergunta >>"
    
    st.button(botao_texto, on_click=avancar_questao, type="secondary", use_container_width=True)


def renderizar_resultado_final():
    """Renderiza a mensagem final."""
    
    total_questoes = len(QUESTOES)
    acertos = st.session_state.acertos
    
    st.balloons() # Efeitos visuais de celebração
    
    st.markdown(f"<h2>✨ Resultado Final: {acertos} acertos de {total_questoes}!</h2>", unsafe_allow_html=True)

    # Mensagem "Amo Você!" estilizada
    st.markdown("""
        <div style="
            text-align: center; 
            margin: 40px 0; 
            padding: 30px; 
            border: 5px double #ff1493; /* Deep Pink */
            border-radius: 15px; 
            background-color: #fff0f5; /* Lavender Blush */
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        ">
            <h1>Amo Você! ❤️</h1>
        </div>
    """, unsafe_allow_html=True)

    # Opcional: botão para recomeçar
    st.button("Recomeçar o Quiz", on_click=iniciar_quiz)


# --- Aplicação Principal (Onde o Streamlit roda) ---

def main():
    st.set_page_config(page_title="Para a Mais Linda", layout="centered")
    
    if st.session_state.status == 'inicio':
        renderizar_tela_inicio()
        
    elif st.session_state.status == 'quiz':
        
        # Se a pessoa clicou em uma resposta (mostrar_popup é True), renderiza o popup
        if st.session_state.mostrar_popup:
            renderizar_popup()
        
        # Caso contrário, mostra a questão atual
        else:
            questao_atual = QUESTOES[st.session_state.indice_questao]
            renderizar_questao(questao_atual)
            
    elif st.session_state.status == 'resultado':
        renderizar_resultado_final()

if __name__ == '__main__':
    main()
