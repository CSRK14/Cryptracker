<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crypto Tracker</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <style>
        body {
            font-family: 'Arial', sans-serif;
        }

        .container {
            max-width: 1200px;
            margin: auto;
        }

        input#search {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>

    <!-- Navigation Bar -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <a class="navbar-brand" href="#">Crypto Tracker</a>
    </nav>

    <!-- Search Bar -->
    <div class="container mt-4">
        <input id="search" class="form-control" type="text" placeholder="Search for a cryptocurrency...">
    </div>

    <!-- Crypto Table -->
    <div class="container mt-4">
        <table class="table table-hover">
            <thead class="thead-dark">
                <tr>
                    <th>Rank</th>
                    <th>Name</th>
                    <th>Symbol</th>
                    <th>Price</th>
                    <th>Market Cap</th>
                    <th>24h Change</th>
                </tr>
            </thead>
            <tbody id="cryptoTable">
                <!-- Data will be populated here -->
            </tbody>
        </table>
    </div>

    <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
    <script>
        // API URL
        const API_URL = 'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd';

        // Fetching data from API
        fetch(API_URL)
            .then(response => response.json())
            .then(data => {
                let cryptoTable = document.getElementById('cryptoTable');
                data.forEach((coin, index) => {
                    let row = `<tr>
                                    <td>${index + 1}</td>
                                    <td>${coin.name}</td>
                                    <td>${coin.symbol.toUpperCase()}</td>
                                    <td>$${coin.current_price.toLocaleString()}</td>
                                    <td>$${coin.market_cap.toLocaleString()}</td>
                                    <td class="${coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger'}">
                                        ${coin.price_change_percentage_24h.toFixed(2)}%
                                    </td>
                                </tr>`;
                    cryptoTable.innerHTML += row;
                });
            })
            .catch(error => console.error('Error fetching data:', error));

        // Search functionality
        document.getElementById('search').addEventListener('keyup', function() {
            let filter = this.value.toUpperCase();
            let rows = document.getElementById('cryptoTable').getElementsByTagName('tr');

            for (let i = 0; i < rows.length; i++) {
                let td = rows[i].getElementsByTagName('td')[1];
                if (td) {
                    let txtValue = td.textContent || td.innerText;
                    if (txtValue.toUpperCase().indexOf(filter) > -1) {
                        rows[i].style.display = "";
                    } else {
                        rows[i].style.display = "none";
                    }
                }       
            }
        });
    </script>
</body>
</html>
