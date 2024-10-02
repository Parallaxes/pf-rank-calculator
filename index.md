---
layout: default
title: Phantom Forces Rank Calculator
---

<h1>Phantom Forces Rank Calculator</h1>

<form id="account-form">
  <table>
    <tr>
      <th>Name</th>
      <th>Rank</th>
      <th>XP</th>
    </tr>
    <tr>
      <td><input type="text" id="name" name="name" required></td>
      <td><input type="number" id="rank" name="rank" min="0"></td>
      <td><input type="number" id="xp" name="xp" min="0"></td>
    </tr>
  </table>
  
  <button type="button" onclick="addAccount()">Add Account</button>
</form>

<table id="accounts-table">
  <thead>
    <tr>
      <th>Name</th>
      <th>Rank</th>
      <th>XP</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

<h2>Total XP: <span id="total-xp">0</span></h2>
<h2>Total Rank: <span id="total-rank">0</span></h2>

<script>
  let accounts = [];

  function calculateXP(rank) {
    return 500 * rank * (rank + 1);
  }

  function calculateRank(xp) {
    return Math.floor(((Math.sqrt(1 + ((4 * xp) / 500))) - 1) / 2);
  }

  function addAccount() {
    const name = document.getElementById('name').value;
    let rank = parseInt(document.getElementById('rank').value);
    let xp = parseInt(document.getElementById('xp').value);

    // Handle NaN cases
    if (!isNaN(rank) && isNaN(xp)) {
        xp = calculateXP(rank);
    } else if (!isNaN(xp) && isNaN(rank)) {
        rank = calculateRank(xp);
    }

    // Only proceed if all inputs are valid
    if (name && !isNaN(rank) && !isNaN(xp)) {
        accounts.push({ name, rank, xp });
        
        // Reset the input fields
        document.getElementById('name').value = '';
        document.getElementById('rank').value = '';
        document.getElementById('xp').value = '';
        
        updateTable();
        updateTotals();
    } else {
        alert('Please provide valid values for name, rank, and XP.');
    }
    }


  function updateTable() {
    const tbody = document.getElementById('accounts-table').getElementsByTagName('tbody')[0];
    tbody.innerHTML = '';
    accounts.forEach(account => {
      const row = tbody.insertRow();
      row.insertCell(0).innerText = account.name;
      row.insertCell(1).innerText = account.rank;
      row.insertCell(2).innerText = account.xp;
    });
  }

  function updateTotals() {
    const totalXP = accounts.reduce((sum, account) => sum + account.xp, 0);
    const totalRank = calculateRank(totalXP);
    document.getElementById('total-xp').innerText = totalXP;
    document.getElementById('total-rank').innerText = totalRank;
  }
</script>
