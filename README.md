<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Desafio Gamificado SENAI</title>
    <style>
        /* ESTILOS (CSS) */
        body {
            font-family: Arial, sans-serif;
            background-color: #2c3e50;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        #game-container {
            width: 900px;
            height: 600px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
            position: relative;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        /* BARRA SENAI */
        .header {
            height: 15px;
            background: linear-gradient(90deg, #005cbb 60%, #e30613 60%);
            width: 100%;
        }

        /* TELAS */
        .screen {
            flex: 1;
            display: none; /* Escondido por padrão */
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 40px;
            text-align: center;
        }

        .screen.active {
            display: flex; /* Mostra a tela ativa */
            animation: fade 0.5s;
        }

        @keyframes fade { from {opacity: 0;} to {opacity: 1;} }

        /* TEXTOS E BOTÕES */
        h1 { color: #005cbb; font-size: 40px; margin-bottom: 10px; }
        h2 { color: #333; margin-bottom: 20px; }
        p { font-size: 20px; color: #666; line-height: 1.5; }

        .btn-start {
            background-color: #005cbb;
            color: white;
            padding: 15px 40px;
            font-size: 24px;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            margin-top: 30px;
        }
        .btn-start:hover { background-color: #004494; }

        /* OPÇÕES DE RESPOSTA */
        .options-container {
            display: grid;
            gap: 15px;
            width: 100%;
            max-width: 700px;
            text-align: left;
        }

        .btn-option {
            background: #f8f9fa;
            border: 2px solid #ddd;
            padding: 20px;
            font-size: 18px;
            cursor: pointer;
            border-radius: 8px;
            transition: 0.2s;
        }

        .btn-option:hover {
            border-color: #005cbb;
            background-color: #eef6ff;
        }

        /* RESULTADO */
        .medal-emoji { font-size: 100px; margin: 20px 0; }
        .badge {
            background: #eee;
            padding: 5px 15px;
            border-radius: 15px;
            font-weight: bold;
            color: #555;
            text-transform: uppercase;
            font-size: 14px;
        }

        /* MODAL DE FEEDBACK */
        #modal {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            z-index: 100;
        }
        #modal-content {
            background: white;
            color: #333;
            padding: 40px;
            border-radius: 10px;
            max-width: 500px;
            text-align: center;
        }
        .btn-next {
            background: #005cbb; color: white; border: none;
            padding: 10px 30px; font-size: 18px; border-radius: 5px;
            cursor: pointer; margin-top: 20px;
        }

    </style>
</head>
<body>

    <div id="game-container">
        <div class="header"></div>

        <!-- TELA 1: INÍCIO -->
        <div id="screen-start" class="screen active">
            <div style="font-size: 80px;">👷</div>
            <h1>Desafio Ferro Forte</h1>
            <p>Você é o Técnico em Segurança responsável.<br>Tome as decisões certas para evitar acidentes.</p>
            <button class="btn-start" onclick="irPara('screen-q1')">COMEÇAR AUDITORIA</button>
        </div>

        <!-- TELA 2: PERGUNTA 1 -->
        <div id="screen-q1" class="screen">
            <div class="badge">Fase 1: Observação</div>
            <h2>⚠️ Risco no Galpão</h2>
            <p>Trabalhadores no telhado sem linha de vida. Empilhadeiras passando embaixo. O que você faz?</p>
            <div class="options-container">
                <button class="btn-option" onclick="responder(1, false)">A. Peço para as empilhadeiras buzinarem.</button>
                <button class="btn-option" onclick="responder(1, false)">B. Subo no telhado para conversar.</button>
                <button class="btn-option" onclick="responder(1, true)">C. Paro a atividade e isolo a área imediatamente (NR-35).</button>
            </div>
        </div>

        <!-- TELA 3: PERGUNTA 2 -->
        <div id="screen-q2" class="screen">
            <div class="badge">Fase 2: Conflito</div>
            <h2>👔 Pressão da Chefia</h2>
            <p>O Gerente diz: "Não pare a produção! Assine a liberação ou assuma o prejuízo financeiro."</p>
            <div class="options-container">
                <button class="btn-option" onclick="responder(2, false)">A. Libero com termo de responsabilidade.</button>
                <button class="btn-option" onclick="responder(2, true)">B. Exerço Direito de Recusa. A vida é prioridade.</button>
                <button class="btn-option" onclick="responder(2, false)">C. Libero, mas fico vigiando.</button>
            </div>
        </div>

        <!-- TELA 4: PERGUNTA 3 -->
        <div id="screen-q3" class="screen">
            <div class="badge">Fase 3: Documentação</div>
            <h2>📄 O PGR Desatualizado</h2>
            <p>O PGR não contempla a nova área de expansão. O RH quer contratar amanhã.</p>
            <div class="options-container">
                <button class="btn-option" onclick="responder(3, true)">A. Atualizo o Inventário de Riscos antes das contratações.</button>
                <button class="btn-option" onclick="responder(3, false)">B. Uso o PGR antigo e reviso depois.</button>
                <button class="btn-option" onclick="responder(3, false)">C. Faço um PPRA rápido.</button>
            </div>
        </div>

        <!-- TELA 5: RESULTADO -->
        <div id="screen-result" class="screen">
            <h1 id="res-title">Resultado</h1>
            <div id="res-emoji" class="medal-emoji"></div>
            <h2 id="res-msg"></h2>
            <p id="res-level" class="badge" style="font-size: 18px; background: #333; color: white;"></p>
            <button class="btn-start" onclick="location.reload()">REINICIAR</button>
        </div>

        <!-- MODAL FEEDBACK -->
        <div id="modal">
            <div id="modal-content">
                <div id="modal-icon" style="font-size: 50px;"></div>
                <h2 id="modal-title"></h2>
                <p id="modal-msg"></p>
                <button class="btn-next" onclick="proximaFase()">CONTINUAR</button>
            </div>
        </div>

    </div>

    <script>
        // LÓGICA DO JOGO
        let pontuacao = 0;
        let perguntaAtual = 1;

        // Função para trocar de tela
        function irPara(idTela) {
            // Esconde todas as telas
            document.querySelectorAll('.screen').forEach(t => t.classList.remove('active'));
            // Mostra a tela desejada
            document.getElementById(idTela).classList.add('active');
        }

        // Função ao clicar na resposta
        function responder(numeroPergunta, acertou) {
            const modal = document.getElementById('modal');
            const icon = document.getElementById('modal-icon');
            const title = document.getElementById('modal-title');
            const msg = document.getElementById('modal-msg');

            modal.style.display = 'flex'; // Mostra o modal

            if (acertou) {
                pontuacao++;
                icon.innerText = "✅";
                title.innerText = "Decisão Correta!";
                title.style.color = "green";
                msg.innerText = getFeedback(numeroPergunta, true);
            } else {
                icon.innerText = "❌";
                title.innerText = "Decisão Arriscada";
                title.style.color = "red";
                msg.innerText = getFeedback(numeroPergunta, false);
            }
        }

        // Textos de feedback
        function getFeedback(q, acertou) {
            if (q === 1) return acertou ? "Isolamento é a prioridade da NR-35." : "Buzinar não impede queda de materiais.";
            if (q === 2) return acertou ? "Segurança jurídica e física vêm primeiro." : "Assumir risco de morte é negligência.";
            if (q === 3) return acertou ? "O PGR deve refletir a realidade atual (NR-01)." : "Documentação desatualizada gera multas.";
        }

        // Avança para a próxima pergunta ou resultado
        function proximaFase() {
            document.getElementById('modal').style.display = 'none'; // Esconde modal
            perguntaAtual++;

            if (perguntaAtual <= 3) {
                irPara('screen-q' + perguntaAtual);
            } else {
                mostrarResultado();
            }
        }

        // Calcula a medalha final
        function mostrarResultado() {
            irPara('screen-result');
            const emoji = document.getElementById('res-emoji');
            const title = document.getElementById('res-title');
            const msg = document.getElementById('res-msg');
            const level = document.getElementById('res-level');

            if (pontuacao === 3) {
                emoji.innerText = "🥇";
                title.innerText = "MEDALHA DE OURO";
                title.style.color = "#FFD700";
                msg.innerText = "Excelente! Você é um especialista em segurança.";
                level.innerText = "Nível 3 - Sênior";
            } else if (pontuacao === 2) {
                emoji.innerText = "🥈";
                title.innerText = "MEDALHA DE PRATA";
                title.style.color = "#7f8c8d";
                msg.innerText = "Bom trabalho, mas atenção aos detalhes.";
                level.innerText = "Nível 2 - Pleno";
            } else {
                emoji.innerText = "🥉";
                title.innerText = "MEDALHA DE BRONZE";
                title.style.color = "#d35400";
                msg.innerText = "Cuidado. Revise as NRs com urgência.";
                level.innerText = "Nível 1 - Júnior";
            }
        }
    </script>

</body>
</html>
