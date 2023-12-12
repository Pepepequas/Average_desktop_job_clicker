# Average_desktop_job_clicker
Is an idle game i made for learning more html

<!doctype html>

<head>
    <title>Programing simulator</title>
    <script src="https://cdn.jsdelivr.net/npm/js-cookie@beta/dist/js.cookie.min.js"></script>
    <script src="https://cdn.jsdelivr.net/gh/erosson/swarm-numberformat@v0.1.0/dist/swarm-numberformat.min.js"></script>
    <script>
        var clicker = {
            scripts: 0,
            upgrades: {
                scripts_machine: {
                    amount: 0,
                    cost: 10,
                    sps: 1,
                    hasun: false,
                    unlocked: 5,
                    name: "defective bot"
                },
                scripts_Computer: {
                    amount: 0,
                    cost: 50,
                    sps: 5,
                    hasun: false,
                    unlocked: 10,
                    name: "Computer"
                },
                scripts_world: {
                    amount: 0,
                    cost: 120,
                    sps: 10,
                    hasun: false,
                    unlocked: 30,
                    name: "Caffeine"
                },
                scripts_rare: {
                    amount: 0,
                    cost: 1500,
                    sps: 10,
                    hasun: false,
                    unlocked: 600,
                    name: "Ask a favor to friend"
                },
                scripts_good: {
                    amount: 0,
                    cost: 4000,
                    sps: 55,
                    hasun: false,
                    unlocked: 1000,
                    name: "Functional bot"
                },
                scripts_god: {
                    amount: 0,
                    cost: 10000,
                    sps: 159,
                    hasun: false,
                    unlocked: 5000,
                    name: "Adrenaline injections"
                },
                scripts_god: {
                    amount: 0,
                    cost: 15000,
                    sps: 359,
                    hasun: false,
                    unlocked: 10000,
                    name: "Grammar corrector"
                }
            },
            achieves: [{ req: "clicker.scripts>0", gotten: false, text: "Best employe for a second" },
            { req: "clicker.scripts>30", gotten: false, text: "Best employe for a minute" },
            { req: "clicker.scripts>60", gotten: false, text: "Best employe for an hour" },
            { req: "clicker.scripts>128", gotten: false, text: "Best employe of the day" },
            { req: "clicker.scripts>1500", gotten: false, text: "Best employe of the week" },
            { req: "clicker.scripts>7000", gotten: false, text: "Best employe of the month!" },
            { req: "clicker.scripts>30000", gotten: false, text: "Best employe of the YEAR!!!" }]
        };
        var CCP = 1;
        var delay = 0;
        var sps = 0;
        function thing_clicked(thing) {
            if (clicker.upgrades[thing].cost <= clicker.scripts) {
                clicker.scripts -= clicker.upgrades[thing].cost;
                clicker.upgrades[thing].amount++;
                clicker.upgrades[thing].cost += Math.round(clicker.upgrades[thing].cost * 0.15);
                update_upgrades();
            }
        }
        function update_upgrades() {
            document.querySelector("#upgrades").innerHTML = "";
            var d = 0;
            for (i in clicker.upgrades) {
                if (clicker.upgrades[i].hasun) {
                    document.querySelector("#upgrades").innerHTML += `<br> <button onclick="thing_clicked('${i}')">${clicker.upgrades
                    [i].name}</button> you have ${numberformat.format(clicker.upgrades[i].amount)}. Cost: ${numberformat.format(clicker.upgrades[i].cost)}`;
                    d += clicker.upgrades[i].sps * clicker.upgrades[i].amount;
                }
            }
            sps = d
        }
        function updatecount() {
            if (Cookies.get("clicker") != null && Cookies.get("clicker") != "undefinied") {
                var clicker1 = JSON.parse(Cookies.get("clicker"));
                for (i in clicker.upgrades) {
                    if (clicker1.upgrades[i] == null) {
                        clicker1.upgrades[i] = clicker.upgrades[i];
                    }
                }
                clicker = clicker1;

                for (i in clicker.achieves) {
                    if (clicker1.achieves[i] == null || clicker.achieves[i].text != clicker1.achieves[i].text) {
                        clicker1.achieves[i] = clicker.achieves[i]
                    }
                }
                clicker = clicker1
            }
            update_upgrades();
            if (Cookies.get("lasttime") != null) {
                var lastsavedate = (Cookies.get("lasttime"));
                lastsavedate = Date.now() - lastsavedate;
                lastsavedate = Math.round(lastsavedate / 1000);
                if (lastsavedate / 60 >= 1) {
                    clicker.scripts += lastsavedate * sps / 1.8;
                    document.querySelector("#achieves").innerHTML += `<br>WHILE YOU WERE NOT WORKING...<br> you got ${numberformat.format
                        (lastsavedate * sps / 1.8)} scripts`;
                }
            }
            setInterval(() => {
                for (i in clicker.upgrades) {
                    clicker.scripts += clicker.upgrades[i].amount * clicker.upgrades[i].sps / 20
                }
                for (i in clicker.achieves) {
                    var b = new Function('return ' + clicker.achieves[i].req);
                    if (b() && !clicker.achieves[i].gotten) {
                        clicker.achieves[i].gotten = true;
                        document.querySelector("#achieves").innerHTML += `<br>ACHEIVEMENT UNLOCKED<br>${clicker.achieves[i].text}`;
                    }
                }
                document.querySelector("#scripts").innerHTML = "you have " + numberformat.format(Number(String(clicker.scripts).split(".")[0])) + " word documents";
                for (i in clicker.upgrades) {
                    if (!clicker.upgrades[i].hasun && clicker.upgrades[i].unlocked <= clicker.scripts) {
                        clicker.upgrades[i].hasun = true;
                        update_upgrades();
                    }
                }
                delay++;
                if (delay >= 40) {
                    Cookies.set("clicker", JSON.stringify(clicker), { expires: 100000 });
                    Cookies.set("lasttime", Date.now(), { expires: 100000 })
                    delay = 0;
                }
            }, 50);
        }
    </script>
</head>

<body onload="updatecount()">
    <h1 id="scripts">you have 0 word documents</h1>
    <button onclick="clicker.scripts += CCP">Create new document</button>
    <div id="upgrades">

    </div>
    <br>
    <div id="achieves" style="width: 400px; height: 300px; overflow:scroll"></div>
</body>
