<h1 align="center">
Popup para controle de cookies
</h1>


## Como usar

1. Copie as tags de SCRIPT e LINK e cole no HEAD do arquivo raiz do site (Ex.: index.html ou .php). 
2. Importe os scripts necessarios para que o BOOTSTRAP funcione, caso seu site já utilize, substitua a tag pela referente mostrada abaixo para atualizar os links.
```
<link
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.1/dist/css/bootstrap.min.css"
  rel="stylesheet"
  integrity="sha384-F3w7mX95PdgyTmZZMECAngseQB83DfGTowi0iMjiWaeVhAn4FJkqJByhZMI3AhiU"
  crossorigin="anonymous"
/>
<script
  src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.1/dist/js/bootstrap.bundle.min.js"
  integrity="sha384-/bQdsTh/da6pkI1MST/rWKFNjaCP5gBSY4sEBT38Q/9RBh9AH40zEOg7Hlq2THRZ"
  crossorigin="anonymous"
></script>
```
3. Agora, copie as funções para o funcionamento do popup. 

```
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
  let ipCliente;
  let user_data;
  const deleteAllCookies = () => {
    const cookiesAll = document.getElementById("AllCookies");

    localStorage.setItem("pol_cookies", 2);
    cookiesAll.style.visibility = "hidden";

    const cookies = document.cookie.split(";");
    for (const cookie of cookies) {
      const eqPos = cookie.indexOf("=");
      const name = eqPos > -1 ? cookie.substr(0, eqPos) : cookie;
      document.cookie = name + "=;expires=Thu, 01 Jan 1970 00:00:00 GMT";
    }
  };
  function checkDisplayAlert() {
    let info = localStorage.getItem("pol_cookies");
    const cookiesAll = document.getElementById("AllCookies");
    if (!info) {
      cookiesAll.style.visibility = "visible";
    } else {
      cookiesAll.style.visibility = "hidden";
      if (info !== "1") {
        deleteAllCookies();
      }
    }
  }
  window.onload = checkDisplayAlert;

  function getIp(callback) {
    function response(s) {
      callback(window.userip);

      s.onload = s.onerror = null;
      document.body.removeChild(s);
    }

    function trigger() {
      window.userip = false;

      var s = document.createElement("script");
      s.async = true;
      s.onload = function () {
        response(s);
      };
      s.onerror = function () {
        response(s);
      };

      s.src = "https://l2.io/ip.js?var=userip";
      document.body.appendChild(s);
    }

    if (/^(interactive|complete)$/i.test(document.readyState)) {
      trigger();
    } else {
      document.addEventListener("DOMContentLoaded", trigger);
    }
  }

  getIp(async function (ip) {
    ipCliente = ip;

    await $.get(
      "http://api.ipstack.com/" +
        ipCliente +
        "?access_key=b48d5abda7b1c59fba9a6f8667470514",
      async function (data, status) {
        console.log(data);
        user_data = data;
      }
    );
  });

  function displayNone() {
    const cookiesAll = document.getElementById("AllCookies");
    cookiesAll.style.visibility = "hidden";
  }

  function displayFlex() {
    const BoxMaisOpcoes = document.getElementById("BoxAllPlusOptions");
    const primaryInfo = document.getElementById("BoxPrimaryInfo");

    BoxMaisOpcoes.style.display = "flex";
    primaryInfo.style.bottom = "191px";
  }

  async function cookiesAcceptYes() {
    localStorage.setItem("pol_cookies", 1);
    var cookies = document.cookie.split(";");

    const domainName = window.location.hostname;
    await $.post(
      "https://api.octo.legal/cookiesAccept?" +
        "ip_cliente=" +
        user_data.ip +
        "&cep=" +
        user_data.zip +
        "&cidade=" +
        user_data.city +
        "&ip_type=" +
        user_data.type +
        "&estado=" +
        user_data.region_code +
        "&idEmpresa=" +
        domainName +
        "&accept=" +
        true +
        "&cookies=" +
        cookies +
        "&token=" +
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbXByZXNhIjoiNjA5M2VkZjkyMWY0NTc1ZGVjZjcxYmNjIiwibmFtZSI6IkxFVkVUIiwiaWF0IjoxNjMyOTE1ODkxfQ.QkrYKuZNIRN4TBi3NOV9pFxV8gTzUIzc1d_F1tqtoYA"
    );
    const cookiesAll = document.getElementById("AllCookies");
    cookiesAll.style.visibility = "hidden";
  }
</script>
 ````
<br />
3. Por final, cole a DIV responsável por exibir o popup  dentro do BODY do arquivo.

```
<div
  id="AllCookies"
  style="
    position: fixed;
    width: 400px;
    max-width: 400px;
    height: 500px;
    max-height: 500px;
    display: flex;
    align-items: center;
    justify-content: center;
    bottom: 0;
    left: 0;
    visibility: hidden;
  "
>
  <div style="width: 50%; max-width: 50%; height: 100%; max-height: 100%">
    <div
      style="
        position: absolute;
        width: 40px;
        height: 40px;
        background: #1c265a;
        bottom: 10px;
        left: 10px;
        border-radius: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: -2px 3px 3px #a9abb4;
      "
    >
      <img
        src="https://octo-imagens.s3.amazonaws.com/6b985db359fec07972312daeba25b763Vector (1).png.png"
        alt="Logo Octo"
        width="30px"
      />
    </div>
  </div>
  <div style="width: 50%; max-width: 50%; height: 100%; max-height: 50%">
    <div
      id="BoxPrimaryInfo"
      style="
        position: absolute;
        width: 650px;
        max-width: 450px;
        height: 200px;
        max-height: 200px;
        background: #ffffff;
        border-radius: 20px;
        bottom: 50px;
        left: 50px;
        box-shadow: -2px 3px 10px #a9abb4;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        padding: 10px;
      "
    >
      <h3
        style="
          font-family: sans-serif;
          font-size: 14px;
          font-weight: 600;
          color: #1c265a;
          height: 25px;
          margin-bottom: 0;
        "
      >
        Controle sua privacidade
      </h3>
      <p
        style="
          font-family: sans-serif;
          font-size: 13px;
          font-weight: 400;
          color: #1c265a;
          margin-bottom: 0;
          text-align: center;
        "
      >
        Nosso site utiliza cookies para melhorar a navegação, personalizar
        anúncio, gerar dados estatísticos e garantir que você tenha a melhor
        experiência durante o uso do site.
      </p>
      <p>
        <a href="#">Politicas de Privacidade</a> -
        <a href="#">Termos de uso</a>
      </p>
      <button
        style="
          width: 95%;
          height: 100%;
          background: #ffffff;
          border: 2px solid #e7e7e7;
          border-radius: 30px;
          color: #1c265a;
          font-family: sans-serif;
          font-size: 14px;
          font-weight: 600;
          cursor: pointer;
          margin-bottom: 1rem;
        "
        data-bs-toggle="modal"
        data-bs-target="#myModal"
      >
        Minhas opções
      </button>
      <div
        style="
          width: 100%;
          display: flex;
          align-items: center;
          justify-content: space-evenly;
        "
      >
        <button
          style="
            width: 45%;
            height: 100%;
            background: #ffffff;
            border: 1px solid #1c265a;
            border-radius: 30px;
            color: #1c265a;
            font-family: sans-serif;
            font-size: 14px;
            font-weight: 400;
            cursor: pointer;
          "
          onclick="deleteAllCookies()"
        >
          Rejeitar todos
        </button>
        <button
          style="
            width: 45%;
            height: 100%;
            border: none;
            border-radius: 30px;
            background: #1c265a;
            color: #ffffff;
            font-family: sans-serif;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
          "
          id="AceitoAll"
          onclick="cookiesAcceptYes()"
        >
          Aceito
        </button>
      </div>
    </div>
  </div>
</div>
<!-- Modal -->
<div class="modal fade" id="myModal" role="dialog">
  <div class="modal-dialog">
    <!-- Modal content-->
    <div class="modal-content">
      <div class="modal-header">
        <h4 class="modal-title">Politica de cookies</h4>
        <button
          type="button"
          class="btn-close"
          data-bs-dismiss="modal"
          aria-label="Close"
        ></button>
      </div>
      <div class="modal-body">
        <div class="form-check">
          <input
            class="form-check-input"
            type="radio"
            name="exampleRadios"
            id="default"
            value="option1"
            disabled
          />
          <label
            style="color: #1c265a; font-weight: 600"
            class="form-check-label"
            for="exampleRadios1"
          >
            Essentials <small>(obrigatório)</small>
          </label>
          <br />
          <small>
            São os cookies estritamente necessários para fornecer nossos
            serviços e para que o nosso site funcione corretamente. Propósito:
            Essenciais
          </small>
        </div>
        <div class="form-check">
          <input
            class="form-check-input"
            type="radio"
            name="flexRadioDefault"
            id="flexRadioDefault2"
            value="option1"
          />
          <label
            style="color: #1c265a; font-weight: 600"
            class="form-check-label"
            for="exampleRadios2"
            checked
            >Opcionais
          </label>
          <br />
          <small>
            Cookies funcionais, ajustam o site a serviços de terceiros, como
            vinculo do seu perfil em redes sociais, comentarios, chatbots e etc.
            <br />
            Propósito: Melhorar a usabilidade no site.
          </small>
        </div>
      </div>
      <div class="modal-footer">
        <button
          style="width: 14rem"
          type="button"
          class="btn btn-secondary"
          data-bs-dismiss="modal"
        >
          Cancelar
        </button>
        <button
          id="AceitoAll"
          style="margin-left: auto; width: 14rem"
          type="button"
          class="btn btn-success"
          data-bs-dismiss="modal"
          onclick="cookiesAcceptYes()"
        >
          Aplicar
        </button>
      </div>
    </div>
  </div>
</div>
```


