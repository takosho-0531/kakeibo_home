# kakeibo_home
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>家族の共有家計簿</title>
    <style>
        body { font-family: sans-serif; background-color: #f0f4f8; padding: 20px; display: flex; justify-content: center; }
        .container { background: white; padding: 20px; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); width: 100%; max-width: 400px; }
        h1 { text-align: center; color: #333; font-size: 22px; }
        .form-group { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
        input, select, button { padding: 12px; font-size: 16px; border: 1px solid #ccc; border-radius: 6px; }
        button { background-color: #007bff; color: white; border: none; font-weight: bold; cursor: pointer; }
        ul { list-style: none; padding: 0; }
        li { background: #f9f9f9; padding: 10px; margin-bottom: 5px; border-radius: 6px; display: flex; justify-content: space-between; border-left: 5px solid #007bff; }
    </style>
</head>
<body>
    <div class="container">
        <h1>👪 家族の共有家計簿</h1>
        <div class="form-group">
            <select id="user">
                <option value="パパ">パパ</option>
                <option value="ママ">ママ</option>
            </select>
            <input type="number" id="amount" placeholder="金額 (円)">
            <select id="category">
                <option value="食費">食費</option>
                <option value="日用品">日用品</option>
                <option value="その他">その他</option>
            </select>
            <button id="add-btn">記録する</button>
        </div>
        <h2>履歴</h2>
        <ul id="history-list"></ul>
    </div>

    <script>
        const amountInput = document.getElementById('amount');
        const categorySelect = document.getElementById('category');
        const userInput = document.getElementById('user');
        const addBtn = document.getElementById('add-btn');
        const historyList = document.getElementById('history-list');

        let records = JSON.parse(localStorage.getItem('family_kakeibo')) || [];

        function updateUI() {
            historyList.innerHTML = '';
            records.forEach(r => {
                const li = document.createElement('li');
                li.innerHTML = `<span>[${r.user}] ${r.category}</span> <span>${Number(r.amount).toLocaleString()} 円</span>`;
                historyList.appendChild(li);
            });
        }

        addBtn.addEventListener('click', () => {
            const amount = amountInput.value;
            const category = categorySelect.value;
            const user = userInput.value;
            if (!amount) return alert('金額を入れてね');
            records.push({ amount, category, user });
            localStorage.setItem('family_kakeibo', JSON.stringify(records));
            amountInput.value = '';
            updateUI();
        });
        updateUI();
    </script>
</body>
</html>
