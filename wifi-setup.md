# WiFi instellen op FullPageOS

## Handmatig via de boot partitie (de "Ghost" methode)

Stop de SD-kaart in je computer en open de schijf `boot`.

Maak een nieuw tekstbestand aan en noem dit exact: `wpa_supplicant.conf`

Plak de volgende inhoud erin en pas de naam en het wachtwoord aan:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=NL

network={
    ssid="NAAM_VAN_WIFI"
    psk="WACHTWOORD"
    key_mgmt=WPA-PSK
}
```

Sla het bestand op en stop de kaart in de Pi. Bij het opstarten leest de Pi dit bestand, neemt de instellingen over en verwijdert het bestand daarna automatisch voor de veiligheid.
