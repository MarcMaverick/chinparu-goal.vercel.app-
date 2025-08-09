# chinparu-goal.vercel.app-
🎯 Ziel der App: Benutzer lädt Begegnungen + Ergebnisse hoch (z. B. als CSV-Datei). App analysiert die Tore der letzten Spiele pro Team. App zeigt pro Mannschaft eine geschätzte Anzahl Tore für ein nächstes Spiel.
};

    matches.forEach((match) => {
      const homeTeam = match["Team Heim"];
      const awayTeam = match["Team Auswärts"];
      const homeGoals = parseInt(match["Tore Heim"], 10);
      const awayGoals = parseInt(match["Tore Auswärts"], 10);

      if (!stats[homeTeam]) stats[homeTeam] = { goals: 0, games: 0 };
      if (!stats[awayTeam]) stats[awayTeam] = { goals: 0, games: 0 };

      stats[homeTeam].goals += homeGoals;
      stats[homeTeam].games += 1;

      stats[awayTeam].goals += awayGoals;
      stats[awayTeam].games += 1;
    });

    const averages = {};
    for (const team in stats) {
      const avg = stats[team].goals / stats[team].games;
      averages[team] = avg.toFixed(2);
    }

    setTeamStats(averages);
  };

  return (
    <div style={{ padding: "20px", maxWidth: "800px", margin: "0 auto" }}>
      <h1>Chinparu Goal ⚽</h1>
      <label style={{ display: "block", marginBottom: "20px" }}>
        <input
          type="file"
          accept=".csv"
          onChange={handleFileUpload}
          style={{ display: "none" }}
        />
        <button
          style={{
            padding: "10px 20px",
            background: "#007bff",
            color: "white",
            border: "none",
            cursor: "pointer",
          }}
        >
          <Upload size={16} style={{ marginRight: "8px" }} />
          CSV-Datei hochladen
        </button>
      </label>

      <div style={{ display: "grid", gap: "10px" }}>
        {Object.entries(teamStats).map(([team, avg]) => (
          <div
            key={team}
            style={{
              background: "white",
              padding: "10px",
              borderRadius: "6px",
              boxShadow: "0 2px 4px rgba(0,0,0,0.1)",
            }}
          >
            <h2>{team}</h2>
            <p>Durchschnittliche Tore: <strong>{avg}</strong></p>
            <p>Prognose: <strong>{Math.round(avg)} Tore</strong></p>
          </div>
        ))}
      </div>
    </div>
  );
}
