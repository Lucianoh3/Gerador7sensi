<!DOCTYPE html>
<html lang="pt-BR" class="bg-black text-white">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>7 Sensi - Gerador</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    #popup { display: flex; }
    @media (min-width: 640px) { .max-w-2xl { max-width: 42rem; } }
  </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center p-4">

  <div id="popup" class="fixed inset- bg/95 z-50 flex items-center justify-center p-6">
    <div class="text-center">
      <h1 class="text-5xl md:text-7xl font-black text-red-600 mb-8 animate-pulse"></h1>
      <button onclick="()" class="bg--600 hover:bg-red-700 text-white text-2xl font-bold py-6 px-12 rounded-xl transition transform hover:scale-110">
        
    </div>
  </div>

  <div class="w-full max-w-2xl space-y-8">
    <div class="text-center mb-8">
      <img src="https://i.imgur.com/EpUnGkB.png" alt="Logo" class="w-40 mx-auto">
      <h1 class="text-3xl font-bold mt-4">Gerador de Sensibilidade</h1>
    </div>

    <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-6">
      <label class="block text-lg font-medium mb-4">Sistema Operacional</label>
      <div class="flex bg-black/30 rounded-lg p-1">
        <button id="androidBtn" class="w-1/2 py-3 rounded-md font-bold bg-red-600">Android</button>
        <button id="iosBtn" class="w-1/2 py-3 rounded-md font-bold text-gray-400">iOS</button>
      </div>
    </div>

    <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-6">
      <label class="block text-lg font-medium mb-4">Modelo do celular</label>
      <input id="modelo" type="text" placeholder="Ex: Galaxy S23, iPhone 14" class="w-full px-4 py-3 bg-black/30 border border-gray-600 rounded-lg focus:ring-2 focus:ring-red-500">
    </div>

    <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-6">
      <label class="block text-lg font-medium mb-4">Escolha o nível</label>
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
        <button onclick="gerar('Baixa')" class="bg-[#a12323] hover:bg-[#b03030] py-5 rounded-lg font-bold text-xl transition hover:scale-105">Baixa</button>
        <button onclick="gerar('Média')" class="bg-[#a12323] hover:bg-[#b03030] py-5 rounded-lg font-bold text-xl transition hover:scale-105">Média</button>
        <button onclick="gerar('Alta')" class="bg-[#a12323] hover:bg-[#b03030] py-5 rounded-lg font-bold text-xl transition hover:scale-105">Alta</button>
      </div>
    </div>

    <div id="loading" class="hidden text-center py-12">
      <div class="w-16 h-16 border-4 border-gray-600 border-t-red-500 rounded-full animate-spin mx-auto"></div>
      <p class="mt-6 text-2xl text-red-500 font-bold">Gerando sensibilidade...</p>
    </div>

    <div id="resultado" class="hidden space-y-6"></div>
  </div>

  <script>
    document.getElementById('popup').style.display = 'flex';
    function abrirCanal() {
      document.getElementById('popup').style.display = 'none';
      window.open
    }

    let os = 'Android';
    document.getElementById('androidBtn').onclick = () => { os = 'Android'; trocar('androidBtn','iosBtn'); }
    document.getElementById('iosBtn').onclick = () => { os = 'iOS'; trocar('iosBtn','androidBtn'); }

    function trocar(ativo, inativo) {
      document.getElementById(ativo).classList.add('bg-red-600','text-white');
      document.getElementById(ativo).classList.remove('text-gray-400');
      document.getElementById(inativo).classList.remove('bg-red-600','text-white');
      document.getElementById(inativo).classList.add('text-gray-400');
    }

    function rand(min, max) { return Math.floor(Math.random() * (max - min + 1)) + min; }
    function randFloat(min, max, dec) { return parseFloat((Math.random() * (max - min) + min).toFixed(dec)); }
    function escolha(arr) { return arr[rand(0, arr.length-1)]; }

    async function gerar(nivel) {
      const modelo = document.getElementById('modelo').value.trim() || (os === 'Android' ? 'Android' : 'iPhone');
      if (!modelo) return alert('Digite o modelo do celular!');

      document.getElementById('loading').classList.remove('hidden');
      document.getElementById('resultado').classList.add('hidden');
      await new Promise(r => setTimeout(r, 4000));

      const base = nivel === 'Baixa' ? {g:rand(70,100),pv:rand(70,100),m2:rand(70,100),m4:rand(70,100),awm:rand(15,25),ol:rand(50,70),bt:rand(65,80), dpi:rand(410,520)}
                 : nivel === 'Média' ? {g:rand(120,150),pv:rand(120,150),m2:rand(120,150),m4:rand(120,150),awm:rand(25,35),ol:rand(60,80),bt:rand(60,75), dpi:rand(450,610)}
                                     : {g:rand(130,200),pv:rand(130,200),m2:rand(130,200),m4:rand(130,200),awm:rand(35,45),ol:rand(70,90),bt:rand(55,70), dpi:rand(620,740)};

      const config = os === 'Android' ? {
        geral: base.g, pontoVermelho: base.pv, mira2x: base.m2, mira4x: base.m4, miraAWM: base.awm, olhadinha: base.ol, botaoAtirar: base.bt,
        dpi: base.dpi,
        velPonteiro: escolha(['Lenta','Normal','Rápida']),
        fonte: escolha(['Pequena','Padrão','Grande']),
        anim: escolha(['0.5x','1x','Desligada']),
        atraso: escolha(['Curto','Médio','Longo']),
        mouse: escolha(['Ativado','Desativado']),
        permanencia: randFloat(0.1,0.5,1)+'s',
        tempoLeitura: randFloat(0.1,0.4,2)+'s',
        atrasoPrimeiro: randFloat(0.1,0.4,2)+'s',
        numeroLeituras: rand(2,10)+' repetições',
        ignorarToques: randFloat(0.01,0.05,2)+'s',
        destaqueLeitura: escolha(['Linha sólida fina','Linha sólida média','Linha sólida espessa']),
        leituraPontual: randFloat(0.2,0.8,2)+' cm por segundo',
        personalizarGestos: escolha(['Focar no primeiro item','Focar no último item','Rolar para frente']),
        reduzirTitulo: escolha(['Ativado','Desativado']),
        filtrarEventos: escolha(['Ativar todas as opções','Desativar tudo','Filtrar por tipo'])
      } : {
        geral: base.g, pontoVermelho: base.pv, mira2x: base.m2, mira4x: base.m4, miraAWM: base.awm, olhadinha: base.ol, botaoAtirar: base.bt,
        cursor: rand(90,120),
        ciclos: rand(3,10),
        tolerancia: escolha(['Padrão','Alta','Baixa']),
        segundosAdaptado: randFloat(1.5,2.5,2)+'s',
        cursorMovel: escolha(['Preciso','Individual']),
        velocidadeCursor: rand(100,120)
      };

      document.getElementById('resultado').innerHTML = `
        <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-8 text-center">
          <h2 class="text-3xl font-bold mb-2">Sensi ${nivel} Gerada!</h2>
          <p class="text-xl text-gray-400">${modelo} • ${os}</p>
        </div>

        <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-6">
          <h3 class="text-2xl font-bold text-red-500 mb-6">Sensi no Jogo</h3>
          <div class="grid grid-cols-2 gap-6 text-xl">
            <div><span class="text-gray-400">Geral:</span> <strong>${config.geral}</strong></div>
            <div><span class="text-gray-400">P. Vermelho:</span> <strong>${config.pontoVermelho}</strong></div>
            <div><span class="text-gray-400">Mira 2x:</span> <strong>${config.mira2x}</strong></div>
            <div><span class="text-gray-400">Mira 4x:</span> <strong>${config.mira4x}</strong></div>
            <div><span class="text-gray-400">Mira AWM:</span> <strong>${config.miraAWM}</strong></div>
            <div><span class="text-gray-400">Olhadinha:</span> <strong>${config.olhadinha}</strong></div>
            <div class="col-span-2"><span class="text-gray-400">Botão de Tiro:</span> <strong>${config.botaoAtirar}%</strong></div>
          </div>
        </div>

        ${os === 'Android' ? `
          <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-6">
            <h3 class="text-2xl font-bold text-red-500 mb-6">Ajustes Gerais do Dispositivo</h3>
            <div class="grid grid-cols-2 gap-6 text-xl">
              <div><span class="text-gray-400">DPI:</span> <strong>${config.dpi}</strong></div>
              <div><span class="text-gray-400">Vel. Ponteiro:</span> <strong>${config.velPonteiro}</strong></div>
              <div><span class="text-gray-400">Tamanho da Fonte:</span> <strong>${config.fonte}</strong></div>
              <div><span class="text-gray-400">Escala Animações:</span> <strong>${config.anim}</strong></div>
              <div><span class="text-gray-400">Atraso ao Tocar:</span> <strong>${config.atraso}</strong></div>
              <div><span class="text-gray-400">Mouse Grande:</span> <strong>${config.mouse}</strong></div>
              <div><span class="text-gray-400">Tempo de Permanência:</span> <strong>${config.permanencia}</strong></div>
            </div>
          </div>

          <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-6">
            <h3 class="text-2xl font-bold text-red-500 mb-6">Headtrick (Acesso com Interruptor)</h3>
            <div class="grid grid-cols-2 gap-6 text-xl">
              <div><span class="text-gray-400">Tempo Leitura Automática:</span> <strong>${config.tempoLeitura}</strong></div>
              <div><span class="text-gray-400">Atraso Primeiro Item:</span> <strong>${config.atrasoPrimeiro}</strong></div>
              <div><span class="text-gray-400">Número de Leituras:</span> <strong>${config.numeroLeituras}</strong></div>
              <div><span class="text-gray-400">Ignorar Toques Repetidos:</span> <strong>${config.ignorarToques}</strong></div>
              <div><span class="text-gray-400">Destaque para Leitura:</span> <strong>${config.destaqueLeitura}</strong></div>
              <div><span class="text-gray-400">Leitura Pontual:</span> <strong>${config.leituraPontual}</strong></div>
            </div>
          </div>

          <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-6">
            <h3 class="text-2xl font-bold text-red-500 mb-6">Secretas (Talkback)</h3>
            <div class="grid grid-cols-2 gap-6 text-xl">
              <div><span class="text-gray-400">Personalizar Gestos:</span> <strong>${config.personalizarGestos}</strong></div>
              <div><span class="text-gray-400">Reduzir Atraso Título:</span> <strong>${config.reduzirTitulo}</strong></div>
              <div><span class="text-gray-400">Filtrar Eventos Acess.:</span> <strong>${config.filtrarEventos}</strong></div>
            </div>
          </div>
        ` : `
          <div class="bg-[#1a1c2c] border border-gray-700/50 rounded-xl p-6">
            <h3 class="text-2xl font-bold text-red-500 mb-6">Ajustes do iPhone</h3>
            <div class="grid grid-cols-2 gap-6 text-xl">
              <div><span class="text-gray-400">Tamanho do Cursor:</span> <strong>${config.cursor}</strong></div>
              <div><span class="text-gray-400">Ciclos:</span> <strong>${config.ciclos}</strong></div>
              <div><span class="text-gray-400">Tolerância ao Movimento:</span> <strong>${config.tolerancia}</strong></div>
              <div><span class="text-gray-400">Segundos (Toque Adaptado):</span> <strong>${config.segundosAdaptado}</strong></div>
              <div><span class="text-gray-400">Cursor Móvel:</span> <strong>${config.cursorMovel}</strong></div>
              <div><span class="text-gray-400">Velocidade do Cursor:</span> <strong>${config.velocidadeCursor}</strong></div>
            </div>
          </div>
        `}
      `;

      document.getElementById('loading').classList.add('hidden');
      document.getElementById('resultado').classList.remove('hidden');
      document.getElementById('resultado').scrollIntoView({ behavior: 'smooth' });
    }
  </script>
</body>
</html>
