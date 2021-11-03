function $(selector) {
    document.querySelector(selector);
}
function $$(selector) {
    document.querySelectorAll(selector);
}

function tablazatkeszito() {
    //  var meret = $("#meret").value;
    const meret = parseInt(document.getElementById("#meret").value);
    const tablazat = document.createElement("table");
    console.log("indul");
    for (let i = 0; i < meret; i++) {
        const sor = document.createElement("tr");
        console.log("ciklusban")
        tablazat.appendChild(sor);
        for (let j = 0; j < meret; j++) {
            const cella = document.createElement("td");
            sor.appendChild(cella);
            cella.innerHTML = `${i * meret + j + 1}`;
            cella.addEventListener('click', function () {
                csere(i * meret + j);
                setTimeout(function () {
                    if (vege()) {
                        alert("Gratulalok nyertel. Szeretned folytatni?");
                        document.getElementById("#tablazat").innerHTML = "";
                        document.querySelector("button").disabled = false;
                    }
                }, 10);
            });

        }
    }
    document.getElementById("#tablazat").appendChild(tablazat);
    const buttons = document.querySelectorAll("td");

    for (var i = 0; i < meret * meret - 1; i++) {
        buttons[i].classList.add("normal");
    }
    buttons[meret * meret - 1].classList.add("fekete");

}

function keres(mit) {
    var x;
    const meret = parseInt(document.getElementById("#meret").value);
    for (let i = 0; i < meret; i++) {
        //console.log("1...." + i);
        for (let j = 0; j < meret; j++) {
            //  console.log("2..." + j);
            if (document.querySelectorAll("td")[i * meret + j].innerHTML == mit) {
                x = parseInt(i * meret + j);
                //console.log("megvagy");
            }
        }
    }
    return x;
}

function kezdo() {
    for (let i = 0; i < 2; i++) {
        osszekever();
    }
}

function osszekever() {

    const meret = parseInt(document.getElementById("#meret").value);
    var fekete = keres(meret * meret);
    var szomszed = szomszedszam();
    var szomszedLista = szomszedok();
    if (szomszed == 2) {
        csere(szomszedLista[Math.floor(Math.random() * 2)]);
    } else if (szomszed == 3) {

        csere(szomszedLista[Math.floor(Math.random() * 3)]);

    } else if (szomszed == 4) {
        csere(szomszedLista[Math.floor(Math.random() * 4)]);
    } else {
        console.log("valami hiba történt");
    }



}

function szomszedszam() {
    var szomszedszam = 0;
    const meret = parseInt(document.getElementById("#meret").value);
    var fekete = keres(meret * meret);

    for (let i = 0; i < meret * meret; i++) {
        if (cserelheto(i, fekete)) {
            szomszedszam++;
        }
    }
    return szomszedszam;
}

function szomszedok() {
    var szomszedai = [];
    const meret = parseInt(document.getElementById("#meret").value);
    var fekete = keres(meret * meret);

    for (let i = 0; i < meret * meret; i++) {
        if (cserelheto(i, fekete)) {
            szomszedai.push(i);
        }
    }
    return szomszedai;
}

const tbd = document.querySelector("button");
tbd.addEventListener('click', function () {
    tablazatkeszito();
    kezdo();
    this.disabled = true;
});


/*
const vmi = document.querySelectorAll("td")
for (let i = 0; i < vmi.length; i++) {
    vmi[i].addEventListener('click', function () { csere(i) })
}
*/
const meret = parseInt(document.getElementById("#meret").value);
const tds = document.querySelectorAll("td");
for (let i = 0; i < tds.length; i++) {
    tds[i].addEventListener('click', function () {
        csere(i);
        if (vege) {
            alert("Gratulalok nyertel. Szeretned folytatni?");
            document.getElementById("#tablazat").innerHTML = "";
            document.querySelector("button").disabled = false;
        }
    })
}


//document.querySelectorAll('td').forEach(e => e.addEventListener("click", function () {
//  csere(e)
//}));


function cserelheto(mit, fekete) {
    const meret = parseInt(document.getElementById("#meret").value);
    if (Math.abs(mit - fekete) == 1 || Math.abs(mit - fekete) == meret) {
        // console.log(fekete % meret);
        // console.log(mit % meret)
        if ((fekete % meret == 0 && mit % meret == meret - 1) || (mit % meret == 0 && fekete % meret == meret - 1)) {  /// mikor van gond
            return false;
        } else {
            return true;
        }
    } else {
        return false;
    }
}

function csere(mit) {
    const meret = parseInt(document.getElementById("#meret").value);

    var fekete = keres(meret * meret);
    if (cserelheto(mit, fekete)) {
        var tmp = document.querySelectorAll("td")[mit].innerHTML;
        document.querySelectorAll("td")[mit].innerHTML = document.querySelectorAll("td")[fekete].innerHTML;
        document.querySelectorAll("td")[fekete].innerHTML = tmp;
        document.querySelectorAll("td")[mit].classList.add("fekete");
        document.querySelectorAll("td")[mit].classList.remove("normal");
        document.querySelectorAll("td")[fekete].classList.remove("fekete");
        document.querySelectorAll("td")[fekete].classList.add("normal")
    }
}

function vege() {
    const tds = document.querySelectorAll("td");
    for (let i = 0; i < tds.length; i++) {
        if (tds[i].innerHTML != (i + 1)) {
            return false;
        }
    }
    return true;
}
