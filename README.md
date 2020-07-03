
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="mycss.css">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css"
        integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
    <title>API</title>
</head>

<body>
    <main id="mymain">
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarTogglerDemo01"
                aria-controls="navbarTogglerDemo01" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarTogglerDemo01">
                <a class="navbar-brand" href="#">APICalls</a>
                <ul class="navbar-nav mr-auto mt-2 mt-lg-0">
                    <li class="nav-item active">
                        <a class="nav-link float-right my-2 my-lg-0" href="#">Home <span
                                class="sr-only">(current)</span></a>
                    </li>

                </ul>
            </div>
        </nav>

        <section class="container d-flex flex-column align-items-center">
            <h1 style="text-align: center ;">Show the countries</h1>

            <div id="newListe">
            </div>

            <input type="text" id="myInput" name="" class="form-control" placeholder="entre the name of the country">
            <div id="parentInfo" class="container d-flex flex-column align-items-center "  >

            </div>
            <p style="text-align: center ;">Once you choose a country click the button to see the information about this
                country</p>

            <button type="button" id="myButton" onclick="" class="btn btn-secondary">Discover this country</button>

            <div id="oldListe" >

            </div>

        </section>
    </main>
    <script>
        //--------initiaise la function
        function ajaxGet(url, callback) {
            var req = new XMLHttpRequest();
            req.open("GET", url);
            req.addEventListener("load", function () {
                if (req.status >= 200 && req.status < 400) {
                    // Appelle la fonction callback en lui passant la réponse de la requête
                    callback(req.responseText);
                } else {
                    console.error(req.status + " " + req.statusText + " " + url);
                }
            });
            req.addEventListener("error", function () {
                console.error("Erreur réseau avec l'URL " + url);
            });
            req.send(null);
        };

        //------------------------------------------brojet#1: aficher les noms de tous les pays 

        /* ajaxGet('https://restcountries.eu/rest/v2/all', function (data) {
             var countries = JSON.parse(data)
             var Liste = document.getElementById("oldListe");
             for (let i = 0; i < countries.length; i++) {
                 var mydiv = document.createElement("DIV");
                 mydiv.className = "btn btn-outline-secondary"
                 mydiv.innerHTML = countries[i].name;
                 mydiv.style.display = "flex";
                 Liste.appendChild(mydiv);
 
             };
         });
 
 */
        //-------------------------------------function va lancer quand il aure changemet dans l'input
        document.getElementById("myInput").oninput = myfunction;
        function myfunction(e) {
            var divnew = document.getElementById("newListe")

            let mypattern = document.getElementById("myInput").value; //------faire les donneés saisies comme RegExp
            var reg = new RegExp(mypattern, "i");
            ajaxGet('https://restcountries.eu/rest/v2/all', function (data) {
                console.log("Je commence le ajaxGet")
                console.log(data)
                var myCountries = JSON.parse(data)
                let listeDeName = []
                for (let i = 0; i < myCountries.length; i++) {
                    listeDeName[i] = myCountries[i].name;
                }
                console.log("J'ai finis le premier FOR")
                console.log(listeDeName)
                if (mypattern.length != 0) {
                    console.log("Je suis dans le premier IF")
                    console.log(reg)
                    divnew.innerHTML = "";        //-- chaque fois on saisit un nouveau lettre il va vider le <div>
                    for (let j = 0; j < listeDeName.length; j++) {
                        if (reg.test(listeDeName[j])) {
                            console.log("Je suis dans le 2eme IF")
                            var mydiv = document.createElement("DIV");
                            mydiv.className = "btn btn-outline-secondary btn-lg btn-light"
                            mydiv.innerHTML = listeDeName[j]

                            divnew.appendChild(mydiv)


                            mydiv.addEventListener('click', (event) => {
                                console.log("je suis dans onclick")
                                
                                ajaxGet('https://restcountries.eu/rest/v2/name/' + event.target.innerHTML, function (payinfo) {
                                    var mapPays = JSON.parse(payinfo)
                                    divnew.innerHTML = "";
                                   var divinfo = document.createElement('div')
                                   divinfo.className= "btn btn-outline-secondary btn-lg btn-light"
                                    var payschoisisinfo = document.createElement('p');
                                    var ledrapeau = document.createElement('img');
                                    
                                    console.log(mapPays)
                                    for( let s = 0; s < mapPays.length; s++ ){
                                   let leName = mapPays[s].name;
                                   let leRegion = mapPays[s].region
                                   let leCapital = mapPays[s].capital;
                                   ledrapeau.src  = mapPays[s].flag;
                                   let sousRegion = mapPays[s].subregion
                                   let monnaies = mapPays[s].currencies[0].name
                                   ledrapeau.style.width = "100px"
                                   ledrapeau.style.height = "100px"
                                   ledrapeau.style.margin = "10px"
                                   ledrapeau.className = "float-left"
                                   payschoisisinfo.className=  "btn btn-outline-secondary btn-lg btn-light"
                                   console.log(monnaies)
                                   payschoisisinfo.innerHTML = "Ce pays s'appele( " + `${leName}`  +" ) <br><br>Sa Capitle " + `${leCapital}` + " <br><br> Ce pays est situé en  " + `${leRegion}`+ "  dans ( " + `${sousRegion}` + " ) "  +"<br><br>La monnaie est:  " +`${monnaies}`                       
                                    divinfo.appendChild(ledrapeau)
                                    divinfo.appendChild(payschoisisinfo)
                                   
                                }
                                document.getElementById('parentInfo').appendChild(divinfo)
                                })

                            })

                        } 
                    }
                }else { divnew.innerHTML = ""; }


            })
        }
    </script>

</body>

</html>
