SELECT t.TrackId, t.TrackTitle
FROM Tracks t
WHERE t.TrackId NOT IN (
    -- Escludi le tracce che hanno almeno un artista con meno di 10 canzoni
    SELECT ta.TrackId
    FROM TrackAuthors ta
    WHERE ta.ArtistId IN (
        SELECT ArtistId
        FROM TrackAuthors
        GROUP BY ArtistId
        HAVING COUNT(*) < 10
    )
)
AND t.TrackId IN (
    -- Include solo tracce che hanno almeno un artista
    SELECT TrackId
    FROM TrackAuthors
);