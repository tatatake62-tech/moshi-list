<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>模試検索</title>
    <style>
        body { font-family: sans-serif; margin: 20px; }
        .error { color: red; font-weight: bold; padding: 10px; border: 1px solid red; display: none; }
        input { padding: 8px; }
        button { padding: 8px; cursor: pointer; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #ccc; padding: 10px; text-align: left; }
        th { background: #f0f0f0; }
    </style>
</head>
<body>
    <h2>模試スケジュール検索</h2>
    <div id="errorMsg" class="error"></div>
    <input type="text" id="searchInput" placeholder="模試名を入力...">
    <button onclick="search()">検索</button>

    <table id="resultTable">
        <thead>
            <tr><th>模試名</th><th>申込開始</th><th>実施日</th><th>成績公開</th></tr>
        </thead>
        <tbody id="tbody"></tbody>
    </table>

    <script>
        let allMoshi = [];
        const fileName = 'moshi.csv'; // ★ここがファイル名！

        // ページが開いた瞬間に読み込む
        fetch(fileName)
            .then(res => {
                if (!res.ok) {
                    throw new Error(`「${fileName}」が見つかりません。GitHubにこの名前でアップされていますか？`);
                }
                return res.text();
            })
            .then(data => {
                const lines = data.split(/\r\n|\n/);
                allMoshi = lines.filter(l => l.trim() !== "").map(line => {
                    const p = line.split(',');
                    return { n: p[0], s: p[1], d: p[2], r: p[3] };
                });
                console.log("読み込み成功");
            })
            .catch(err => {
                const errDiv = document.getElementById('errorMsg');
                errDiv.innerText = err.message;
                errDiv.style.display = 'block';
            });

        function search() {
            const q = document.getElementById('searchInput').value.trim();
            const tbody = document.getElementById('tbody');
            tbody.innerHTML = "";
            const results = allMoshi.filter(m => m.n.includes(q));
            results.forEach(m => {
                tbody.innerHTML += `<tr><td>${m.n}</td><td>${m.s}</td><td>${m.d}</td><td>${m.r}</td></tr>`;
            });
        }
    </script>
</body>
</html>
