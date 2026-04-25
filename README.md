[moshi.csv](https://github.com/user-attachments/files/27084281/moshi.csv)
[index.html](https://github.com/user-attachments/files/27084282/index.html)<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>模試管理システム</title>
    <style>
        body { font-family: 'Segoe UI', 'Yu Gothic UI', sans-serif; margin: 20px; background: #f5f7fa; }
        .container { max-width: 800px; margin: 0 auto; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        input { padding: 10px; width: 70%; border: 1px solid #ddd; border-radius: 5px; }
        button { padding: 10px 20px; background: #007bff; color: white; border: none; border-radius: 5px; cursor: pointer; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #eee; padding: 12px; text-align: center; }
        th { background: #007bff; color: white; }
        tr:nth-child(even) { background: #fafafa; }
    </style>
</head>
<body>
    <div class="container">
        <h2>模試スケジュール検索</h2>
        <input type="text" id="searchInput" placeholder="全統 など入力...">
        <button onclick="search()">検索</button>
        <table id="resultTable">
            <thead>
                <tr><th>模試名</th><th>申込開始</th><th>実施日</th><th>成績公開</th></tr>
            </thead>
            <tbody id="tbody"></tbody>
        </table>
    </div>

    <script>
    let allMoshi = [];

    // CSVファイルを読み込む（エラーチェック付き）
    fetch('moshi.csv')
        .then(res => {
            if (!res.ok) throw new Error("CSVファイルが見つかりません！ファイル名を確認して！");
            return res.text();
        })
        .then(data => {
            console.log("読み込んだデータ:", data); // ブラウザのF12キーで見れるログ
            const lines = data.trim().split('\n');
            allMoshi = lines.map(line => {
                const parts = line.split(',');
                return { 
                    name: parts[0] ? parts[0].trim() : "", 
                    start: parts[1] ? parts[1].trim() : "", 
                    date: parts[2] ? parts[2].trim() : "", 
                    res: parts[3] ? parts[3].trim() : "" 
                };
            });
            alert("データの読み込みに成功しました！検索してみてください。");
        })
        .catch(err => {
            alert(err.message); // エラーがあれば画面に警告を出す
        });

    function search() {
        const query = document.getElementById('searchInput').value;
        const tbody = document.getElementById('tbody');
        tbody.innerHTML = "";

        const results = allMoshi.filter(m => m.name.includes(query));
        
        if (results.length === 0) {
            tbody.innerHTML = "<tr><td colspan='4'>見つかりませんでした</td></tr>";
            return;
        }

        results.forEach(m => {
            const row = `<tr><td>${m.name}</td><td>${m.start}</td><td>${m.date}</td><td>${m.res}</td></tr>`;
            tbody.innerHTML += row;
        });
    }
</script>
</body>
</html>
