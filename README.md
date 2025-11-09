# Curriculo
Um gerador de currículos em PHP, HTML, CSS e JavaScript com Bootstrap e jQuery.
Tem como função obter os dados pessoais e profissionais dos usuários (nome, telefone, email, experiências profissionais e habilidades).
Algumas dinâmicas adicionadas via jQuery e uma incrementada no visual com Bootstrap icons.
Opções de imprimir ou salvar em PDF diretamente no navegador.
HTML - Estrutura do conteúdo,
CSS e Bootstrap - Estilização e layout responsivo,
JavaScript e jQuery - Manipulação dinâmica de campos,
Bootstrap icons - ícones visuais no layout,
Função window.print() para abrir impressão no navegador ou salvar como PDF.

<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Gerador de Currículo</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css" rel="stylesheet">

  <script src="https://code.jquery.com/jquery-3.6.4.min.js"></script>

  <style>
    body {
      background: linear-gradient(135deg, #f0f4ff, #e6ecff);
      min-height: 100vh;
      font-family: 'Segoe UI', sans-serif;
    }
    .container {
      background: #fff;
      padding: 40px;
      border-radius: 20px;
      box-shadow: 0 5px 25px rgba(0,0,0,0.1);
      margin-top: 40px;
      max-width: 900px;
    }
    h2 {
      color: #1e3a8a;
      font-weight: 700;
    }
    .form-control, textarea {
      border-radius: 10px;
      border: 1px solid #d0d7ff;
    }
    .btn {
      border-radius: 10px;
    }
    hr {
      border-top: 2px solid #d0d7ff;
      margin: 25px 0;
    }
    #curriculo {
      background: #fdfdfd;
      border-radius: 10px;
      padding: 30px;
      box-shadow: 0 0 10px rgba(0,0,0,0.05);
    }
    @media print {
      .no-print { display: none !important; }
      body { background: #fff; }
    }
  </style>
</head>
<body>

<div class="container">
  <h2 class="text-center mb-4"><i class="bi bi-person-badge-fill"></i> Gerador de Currículo</h2>
  <p class="text-center text-muted mb-4">Preencha suas informações e gere um currículo estilizado pronto para impressão ou download em PDF.</p>

  <form id="form-curriculo">
    <h4 class="text-primary"><i class="bi bi-person-circle"></i> Dados Pessoais</h4>
    <div class="mb-3">
      <label><strong>Nome Completo:</strong></label>
      <input type="text" name="nome" class="form-control" required>
    </div>
    <div class="row">
      <div class="col-md-6 mb-3">
        <label><strong>Email:</strong></label>
        <input type="email" name="email" class="form-control" required>
      </div>
      <div class="col-md-6 mb-3">
        <label><strong>Telefone:</strong></label>
        <input type="text" name="telefone" class="form-control" required>
      </div>
    </div>

    <hr>
    <h4 class="text-primary"><i class="bi bi-mortarboard"></i> Formação Acadêmica</h4>
    <div id="formacoes">
      <div class="formacao mb-3">
        <input type="text" name="formacao[]" class="form-control" placeholder="Ex: Ensino Médio - Escola X (2020)" required>
      </div>
    </div>
    <button type="button" class="btn btn-outline-primary btn-sm mb-3 no-print" id="addFormacao">
      <i class="bi bi-plus-circle"></i> Adicionar Formação
    </button>

    <hr>
    <h4 class="text-primary"><i class="bi bi-briefcase-fill"></i> Experiência Profissional</h4>
    <div id="experiencias">
      <div class="experiencia mb-3">
        <input type="text" name="experiencia[]" class="form-control" placeholder="Ex: Cargo - Empresa Y (2021 - 2023)" required>
      </div>
    </div>
    <button type="button" class="btn btn-outline-primary btn-sm mb-3 no-print" id="addExperiencia">
      <i class="bi bi-plus-circle"></i> Adicionar Experiência
    </button>

    <hr>
    <h4 class="text-primary"><i class="bi bi-lightbulb"></i> Habilidades</h4>
    <textarea name="habilidades" class="form-control mb-3" rows="3" placeholder="Ex: Comunicação, Liderança, Programação..."></textarea>

    <div class="text-center no-print mt-4">
      <button type="submit" class="btn btn-success px-4 py-2"><i class="bi bi-file-earmark-person-fill"></i> Gerar Currículo</button>
    </div>
  </form>

  <div id="curriculo-gerado" class="mt-5" style="display:none;">
    <div id="curriculo"></div>

    <div class="text-center no-print mt-4">
      <button class="btn btn-primary px-4 py-2 me-2" onclick="printCurriculo()">
        <i class="bi bi-printer-fill"></i> Baixar / Imprimir PDF
      </button>
      <button class="btn btn-secondary px-4 py-2" onclick="window.location.reload()">
        <i class="bi bi-arrow-repeat"></i> Novo Currículo
      </button>
    </div>
  </div>
</div>

<script>
$(document).ready(function(){

  $('#addFormacao').click(function(){
    $('#formacoes').append(`
      <div class="formacao mb-3">
        <input type="text" name="formacao[]" class="form-control" placeholder="Ex: Curso - Instituição" required>
      </div>
    `);
  });

  $('#addExperiencia').click(function(){
    $('#experiencias').append(`
      <div class="experiencia mb-3">
        <input type="text" name="experiencia[]" class="form-control" placeholder="Ex: Cargo - Empresa (Ano)" required>
      </div>
    `);
  });

  $('#form-curriculo').on('submit', function(e){
    e.preventDefault();
    const dados = $(this).serializeArray();
    let obj = {};
    dados.forEach(item => {
      if (!obj[item.name]) obj[item.name] = [];
      obj[item.name].push(item.value);
    });

    let html = `
      <h2 class="text-center text-primary mb-1">${obj.nome}</h2>
      <p class="text-center text-muted mb-3">${obj.email} | ${obj.telefone}</p>
      <hr>
      <h4 class="text-secondary"><i class="bi bi-mortarboard"></i> Formação Acadêmica</h4>
      <ul>${obj['formacao[]'].map(f => `<li>${f}</li>`).join('')}</ul>
      <h4 class="text-secondary mt-3"><i class="bi bi-briefcase"></i> Experiência Profissional</h4>
      <ul>${obj['experiencia[]'].map(e => `<li>${e}</li>`).join('')}</ul>
      <h4 class="text-secondary mt-3"><i class="bi bi-lightbulb"></i> Habilidades</h4>
      <p>${obj.habilidades}</p>
    `;

    $('#curriculo').html(html);
    $('#form-curriculo').hide();
    $('#curriculo-gerado').fadeIn();
  });
});

function printCurriculo() {
  const conteudo = document.getElementById('curriculo').innerHTML;
  const janela = window.open('', '_blank');
  janela.document.write(`
    <html>
    <head>
      <title>Currículo</title>
      <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
      <style>
        body { padding: 25px; font-family: Arial, sans-serif; }
        h2, h4 { text-align: center; color: #1e3a8a; }
        ul { margin-left: 20px; }
      </style>
    </head>
    <body>${conteudo}</body>
    </html>
  `);
  janela.document.close();
  janela.print();
}
</script>

</body>
</html>
