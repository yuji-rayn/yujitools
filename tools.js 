const axios = require("axios");
const fs = require("fs");
const path = require("path");
const readline = require("readline");
const cfonts = require("cfonts");
const FormData = require("form-data");
const cheerio = require("cheerio");
const sharp = require("sharp");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

// ===== LOGIN CONFIG =====
const LOGIN_USER = "share";
const LOGIN_PASS = "sharenih";
const LOGIN_KEY = "yuji";

function login() {
console.clear()
  rl.question("👤 Username: ", (user) => {
    rl.question("🔑 Password: ", (pass) => {
      rl.question("🗝️ Key: ", (key) => {
        if (user === LOGIN_USER && pass === LOGIN_PASS && key === LOGIN_KEY) {
          console.log("\n✅ Login berhasil!\n");
          showTitle();
          promptMenu();
        } else {
          console.log("\n❌ Login gagal! Program dihentikan.\n");
          process.exit(0);
        }
      });
    });
  });
}
login()

const OUTPUT_DIR = "/sdcard/hasil-download";
const API_KEY = "yujixd";

// Spotify Configuration
process.env['SPOTIFY_CLIENT_ID'] = '4c4fc8c3496243cbba99b39826e2841f';
process.env['SPOTIFY_CLIENT_SECRET'] = 'd598f89aba0946e2b85fb8aefa9ae4c8';

// VirusTotal Configuration
const VT_API_KEY = "928dc6c89cb000ee255b99634434cb6582ca31cf249620edfb4bb6bd89bcf072";
const API_ENDPOINT_FILES = "https://www.virustotal.com/api/v3/files";
const API_ENDPOINT_URLS = "https://www.virustotal.com/api/v3/urls";
const API_ENDPOINT_ANALYSES = "https://www.virustotal.com/api/v3/analyses";

function showTitle() {
  console.clear();
  cfonts.say('YUJI', {
    font: 'block',
    align: 'center',
    gradient: ['blue', 'cyan'],
    transitionGradient: true,
    env: 'node'
  });
  cfonts.say('TOOLS', {
    font: 'console',
    align: 'center',
    gradient: ['green', 'cyan'],
    transitionGradient: true,
    env: 'node'
  });
}

function sanitizeFilename(name) {
  return name.replace(/[^a-zA-Z0-9-_.]/g, "_");
}

function pickRandom(...list) {
  return list[Math.floor(Math.random() * list.length)];
}

function formatFileSize(bytes) {
  if (bytes === 0) return '0 Bytes';
  const k = 1024;
  const sizes = ['Bytes', 'KB', 'MB', 'GB'];
  const i = Math.floor(Math.log(bytes) / Math.log(k));
  return parseFloat(((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i]);

}

function convert(ms) {
  var minutes = Math.floor(ms / 60000);
  var seconds = ((ms % 60000) / 1000).toFixed(0);
  return minutes + ':' + (seconds < 10 ? '0' : '') + seconds;
}

function promptMenu() {
  const greenBold = "\x1b[1;92m";  // Bold + Bright Green
  const reset = "\x1b[0m";         // Reset styling
  const redBold = '\x1b[91;1m'
  const blue = '\x1b[96;1m'
  
  console.log(greenBold);
  console.log("\n" + "=".repeat(56));
  console.log(" ".repeat(20) + "MENU UTAMA" + " ".repeat(20));
  console.log("=".repeat(56));
  console.log("╔════════════════════════════════════════════════════╗");
  console.log("║ {  1  } TikTok Downloader".padEnd(53) + "║");
  console.log("║ {  2  } Instagram Downloader".padEnd(53) + "║");
  console.log("║ {  3  } Music Downloader".padEnd(53) + "║");
  console.log("║ {  4  } Spotify Downloader".padEnd(53) + "║");
  console.log("║ {  5  } YouTube Transcript".padEnd(53) + "║");
  console.log("║ {  6  } Top Up DANA".padEnd(53) + "║");
  console.log("║ {  7  } NGL Spam".padEnd(53) + "║");
  console.log("║ {  8  } OTP Spam".padEnd(53) + "║");
  console.log("║ {  9  } VirusTotal Scan".padEnd(53) + "║");
  console.log("║ { 10  } Telegram Channel Search".padEnd(53) + "║");
  console.log("║ { 11  } TempMail (Email Sementara)".padEnd(53) + "║");
  console.log("║ { 12  } Verif SMS (Nomor Gratis)".padEnd(53) + "║");
  console.log("║ { 13  } Top Roblox Games".padEnd(53) + "║");
  console.log("║ { 14  } Info Cuaca BMKG".padEnd(53) + "║");
  console.log("║ { 15  } Info KRL Commuter".padEnd(53) + "║");
  console.log("║ { 16  } Info GCam Support".padEnd(53) + "║");
  console.log("║ { 17  } AI Music Generator".padEnd(53) + "║");
  console.log("║ { 18  } Multi-Platform Downloader".padEnd(53) + "║");
  console.log("║ {  0  } Keluar".padEnd(53) + "║");
  console.log("╚════════════════════════════════════════════════════╝");
  console.log("=".repeat(56));

  console.log(reset);
  rl.question("Pilih menu: ", (choice) => {
    switch(choice) {
      case "1": downloadTikTok(); break;
      case "2": downloadInstagram(); break;
      case "3": musicMenu(); break;
      case "4": spotifyMenu(); break;
      case "5": transcriptMenu(); break;
      case "6": topupDana(); break;
      case "7": nglSpamMenu(); break;
      case "8": otpSpamMenu(); break;
      case "9": virusTotalMenu(); break;
      case "10": telegramMenu(); break;
      case "11": tempmailMenu(); break;
      case "12": lubanMenu(); break;
      case "13": robloxMenu(); break;
      case "14": cuacaMenu(); break;
      case "15": krlMenu(); break;
      case "16": gcamMenu(); break;
      case "17": aiMusicMenu(); break;
      case "18": dlpandaMenu(); break;
      case "0":
        console.log("👋 Sampai jumpa!");
        rl.close();
        break;
      default:
        console.log(redBold);
        console.log("❌ Pilihan tidak valid");
        console.log(reset)
        promptMenu();
    }
  });
}
// =================== Roblox Functions ===================
const roblox = {
  _misc: {
    async hit(hitDescription, url, options, returnType = "text") {
      try {
        const response = await axios({
          url,
          method: options.method || 'GET',
          data: options.body,
          headers: options.headers,
          responseType: returnType === "buffer" ? "arraybuffer" : returnType
        });
        if (response.status < 200 || response.status >= 300) {
          throw new Error(`${response.status} ${response.statusText} ${(response.data?.toString()?.substring(0, 100) || `(respond body kosong)`)}...`);
        }
        
        let data;
        if (returnType === "text") {
          data = response.data.toString();
        } else if (returnType === "json") {
          data = typeof response.data === 'string' ? JSON.parse(response.data) : response.data;
        } else if (returnType == "buffer") {
          data = Buffer.from(response.data);
        } else {
          throw new Error(`invalid param return type. pilih text/json/buffer`);
        }
        
        return { data, response };
      } catch (error) {
        throw new Error(`gagal hit. ${hitDescription}.\n${error.message}`);
      }
    },

    log(logText) {
      if (this.debug) console.log(logText);
    },

    customMappingNumber(input) {
      const char = ['𝟢', '𝟣', '𝟤', '𝟥', '𝟦', '𝟧', '𝟨', '𝟩', '𝟪', '𝟫'];
      return input.split("").map(v => isNaN(v) ? v : char[v]).join("");
    }
  },

  debug: false,

  async getGameList(sortId = "top-playing-now") {
    const validSortId = ["top-playing-now"];
    if (!validSortId.includes(sortId)) throw new Error(`invalid sortId. sortId tersedia: ${validSortId.join(", ")}`);

    // hit #1
    const api1 = new URL(`https://apis.roblox.com/explore-api/v1/get-sort-content`);
    api1.search = new URLSearchParams({
      "sessionId": "17996246-1290-440d-b789-d49484115b9a",
      "sortId": sortId,
      "cpuCores": "8",
      "maxResolution": "1920x1080",
      "maxMemory": "8192",
      "networkType": "4g"
    });
    
    const { data: json1 } = await this._misc.hit(`top playing now`, api1.toString(), { method: 'get' }, `json`);
    const gameList = json1?.games?.slice(0, 10);
    if (!gameList?.length) throw new Error(`lah gamelist nya kosong`);
    this._misc.log('hit api 1');

    // hit#2
    const payload = gameList.map(v => ({
      "type": "GameIcon",
      "targetId": v.universeId,
      "format": "webp",
      "size": "256x256",
    }));
    
    const { data: json2 } = await this._misc.hit(`batch download thumbnail`, 'https://thumbnails.roblox.com/v1/batch', { 
      method: 'post',
      body: JSON.stringify(payload),
      headers: { 'Content-Type': 'application/json' }
    }, `json`);
    
    const thumbnaliList = json2.data;
    this._misc.log('hit api 2');

    // gabung value
    const result = gameList.map((v, i) => {
      const thumb = thumbnaliList[i];
      return { ...v, ...thumb };
    });

    return result;
  },

  async serialize(gameListObject) {
    // download semua thumbnail sebagai buffer
    const imageFetchs = gameListObject.map(v => 
      this._misc.hit(`download gambar ${v.name}`, v.imageUrl, {}, 'buffer')
    );
    const imageResults = await Promise.all(imageFetchs);
    const imageBuffers = imageResults.map(v => v.data);

    // gabungkan semua thumbnail menjadi 1 image besarrrrr
    const imageBuffer = await sharp(
      imageBuffers,
      {
        join: {
          across: 5,        // kolom
          shim: 10,         // jarak antar gambar (padding internal)
          background: { r: 255, g: 255, b: 255, alpha: 1 } // warna padding
        }
      })
      .extend({ // padding global di luar grid
        top: 20,
        bottom: 20,
        left: 20,
        right: 20,
        background: { r: 255, g: 255, b: 255, alpha: 1 }
      })
      .png()
      .toBuffer();

    // bikin caption
    const hider = `top 10 playing now games on roblox\n\n`;
    const top10 = gameListObject.map((v, i) => {
      return (i + 1) + " | " + v.name + "\n" +
        "👥 player count " + this._misc.customMappingNumber(v.playerCount.toLocaleString("id-ID")) + "\n" +
        "👍 likes " + this._misc.customMappingNumber((v.totalUpVotes / (v.totalUpVotes + v.totalDownVotes) * 100).toFixed()) + "%\n" +
        "🎮 play now https://www.roblox.com/games/" + v.rootPlaceId;
    }).join("\n\n");

    const caption = hider + top10;
    return { imageBuffer, caption };
  },

  async cek() {
    const gameListObject = await this.getGameList();
    const result = await this.serialize(gameListObject);
    return result;
  }
};

function robloxMenu() {
  console.clear()
  console.log("\n🎮 TOP ROBLOX GAMES");
  console.log("1. 🔍 Lihat Top 10 Game");
  console.log("2. 📥 Download Info Game");
  console.log("3. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", async (choice) => {
    if (choice === "1") {
      try {
        console.log("🔍 Mengambil data game terpopuler di Roblox...");
        const gameList = await roblox.getGameList();
        console.clear()
        console.log("\n🎮 TOP 10 GAME ROBLOX:");
        console.log("=".repeat(60));
        gameList.forEach((game, index) => {
          console.log(`${index + 1}. ${game.name}`);
          console.log(`   👥 Players: ${game.playerCount.toLocaleString()}`);
          console.log(`   👍 Likes: ${((game.totalUpVotes / (game.totalUpVotes + game.totalDownVotes)) * 100).toFixed(0)}%`);
          console.log(`   🎮 Play: https://www.roblox.com/games/${game.rootPlaceId}`);
          console.log("-".repeat(40));
        });
      } catch (error) {
        console.error("❌ Error:", error.message);
      }
      robloxMenu();
    } else if (choice === "2") {
      try {
        console.log("📥 Mengunduh info game...");
        const result = await roblox.cek();
        
        const filename = `roblox_top10.png`;
        const filePath = path.join(OUTPUT_DIR, filename);
        fs.writeFileSync(filePath, result.imageBuffer);
        
        const captionFilename = `roblox_top10.txt`;
        const captionPath = path.join(OUTPUT_DIR, captionFilename);
        fs.writeFileSync(captionPath, result.caption);
        
        console.log(`✅ Berhasil disimpan:\n- Gambar: ${filePath}\n- Caption: ${captionPath}`);
      } catch (error) {
        console.error("❌ Error:", error.message);
      }
      robloxMenu();
    } else if (choice === "3") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      robloxMenu();
    }
  });
}

// =================== BMKG Weather Functions ===================
const cuaca = {
  url: {
    api_search_geo: `https://cuaca.bmkg.go.id/api/df/v1/adm/search`,
    api_search_geo_2: `https://www.gps-coordinates.net/geoproxy`,
    api_cuaca: `https://weather.bmkg.go.id/api/presentwx/coord`,
    api_cuaca_warning: `https://cuaca.bmkg.go.id/api/v1/public/weather/warning`,
  },

  string: {
    gps: '9416bf2c8b1d4751be6a9a9e94ea85ca',
    bmkg: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjFjNWFkZWUxYzY5MzM0NjY2N2EzZWM0MWRlMjBmZWZhNDcxOTNjYzcyZDgwMGRiN2ZmZmFlMWVhYjcxZGYyYjQiLCJpYXQiOjE3MDE1ODMzNzl9.D1VNpMoTUVFOUuQW0y2vSjttZwj0sKBX33KyrkaRMcQ'
  },

  baseHeaders: {
    'accept-encoding': 'gzip, deflate, br, zstd',
  },

  validateCoordinate: function (what, input, startLimit, endLimit) {
    let lat = parseFloat(input);
    if (isNaN(lat) || !(lat >= startLimit && lat <= endLimit)) throw new Error(`${what}`);
  },

  validasiString: function (deskripsi, variabel) {
    if (typeof (variabel) !== "string" || !variabel?.toString()?.trim().length) throw new Error(`param ${deskripsi} harus string/number dan gak boleh kosong!`);
  },

  mintaJson: async function (description, url, fetchOptions) {
    try {
      const response = await axios({
        url,
        method: fetchOptions.method || 'GET',
        data: fetchOptions.body,
        headers: fetchOptions.headers
      });
      if (response.status < 200 || response.status >= 300) {
        throw new Error(`${response.status} ${response.statusText}\n${response.data}`);
      }
      return response.data;
    } catch (error) {
      throw new Error(`gagal minta json: ${description}\nerror: ${error.message}`);
    }
  },

  cariKoordinat: async function (lokasiKamu) {
    "use strict";
    this.validasiString(`lokasi`, lokasiKamu);
    const new_url = new URL(`https://www.google.com/s`);
    new_url.search = new URLSearchParams({
      "tbm": "map",
      "gs_ri": "maps",
      "suggest": "p",
      "authuser": "0",
      "hl": "en",
      "gl": "id",
      "psi": "2OKJaLzkJKOe4-EPttbSoQQ.1753866977195.1",
      "q": lokasiKamu,
      "ech": "22",
      "pb": "!2i22!4m12!1m3!1d130622.22!2d22.22!3d-22.22!2m3!1f0!2f0!3f0!3m2!1i477!2i636!4f13.1!7i20!10b1!12m24!1m5!18b1!30b1!31m1!1b1!34e1!2m3!5m1!6e2!20e3!10b1"+
      "!12b1!13b1!16b1!17m1!3e1!20m4!5e2!6b1!8b1!14b1!46m1!1b0!96b1!19m4!2m3!1i360!2i120!4i8!20m57!2m2!1i203!2i100!3m2!2i4!5b1!6m6!1m2!1i86!2i86!1m2!1i408!2i240!7m33"+
      "!1m3!1e1!2b0!3e3!1m3!1e2!2b1!3e2!1m3!1e2!2b0!3e3!1m3!1e8!2b0!3e3!1m3!1e10!2b0!3e3!1m3!1e10!2b1!3e2!1m3!1e10!2b0!3e4!1m3!1e9!2b1!3e2!2b1!9b0!15m8!1m7!1m2!1m1!1e2"+
      "!2m2!1i195!2i195!3i20!22m3!1s2OKJaLzkJKOe4-EPttbSoQQ!7e81!17s2OKJaLzkJKOe4-EPttbSoQQ:79!23m2!4b1!10b1!24m112!1m32!13m9!2b1!3b1!4b1!6i1!8b1!9b1!14b1!20b1!25b1!18m21"+
      "!3b1!4b1!5b1!6b1!9b1!12b1!13b1!14b1!17b1!20b1!21b1!22b1!25b1!27m1!1b0!28b0!32b1!33m1!1b1!34b1!36e2!10m1!8e3!11m1!3e1!14m1!3b0!17b1!20m2!1e3!1e6!24b1!25b1!26b1!27b1"+
      "!29b1!30m1!2b1!36b1!37b1!39m3!2m2!2i1!3i1!43b1!52b1!54m1!1b1!55b1!56m1!1b1!61m2!1m1!1e1!65m5!3m4!1m3!1m2!1i224!2i298!72m22!1m8!2b1!5b1!7b1!12m4!1b1!2b1!4m1!1e1!4b1!8m10"+
      "!1m6!4m1!1e1!4m1!1e3!4m1!1e4!3sother_user_google_review_posts__and__hotel_and_vr_partner_review_posts!6m1!1e1!9b1!89b1!98m3!1b1!2b1!3b1!103b1!113b1!114m3!1b1!2m1!1b1!117b1"+
      "!122m1!1b1!125b0!126b1!127b1!26m4!2m3!1i80!2i92!4i8!34m19!2b1!3b1!4b1!6b1!8m6!1b1!3b1!4b1!5b1!6b1!7b1!9b1!12b1!14b1!20b1!23b1!25b1!26b1!31b1!37m1!1e81!47m0!49m10!3b1!6m2!1b1!2b1!7m2!1e3!2b1!8b1!9b1!10e2!61b1!67m5!7b1!10b1!14b1!15m1!1b0!69i742"
    });
    
    const response = await axios.get(new_url.toString(), {headers: this.baseHeaders});
    if (response.status < 200 || response.status >= 300) {
      throw new Error(`${response.status} ${response.statusText}. google maps not ok!`);
    }
    
    const data = response.data;
    const hasil = data.split("\n")[1].trim();
    const ar = eval(hasil);

    const flatArray = [...new Set(ar.flat(7).filter(v => v))];
    const dumpKoordinat = flatArray.filter(v => typeof (v) != "string" && !Number.isInteger(v));
    const latitude = dumpKoordinat[0];
    const longitude = dumpKoordinat[1];
    const dumpPlace = flatArray.filter(v => typeof (v) === "string");
    const placeName = dumpPlace[1].split(", ")[0];
    const result = { placeName, latitude, longitude };
    
    if (!longitude || !latitude) throw new Error(`gagal mendapatkan koordinat ${lokasiKamu}`);
    return result;
  },

  getkWeatherByCoordinateBMKG: async function (latitude, longitude, placeName = "") {
    try {
      this.validateCoordinate(`latitude`, latitude, -12, 7);
      this.validateCoordinate(`longitude`, longitude, 93, 142);
    } catch (error) {
      throw new Error("aduh... gak ada data cuaca... " + error.message + "nya kejauhan wkwk");
    }

    const padEnd = 0;
    const namaTempat = placeName.trim().length ? "📌 nama: ".padEnd(padEnd) + placeName + '\n' : '';

    const cuacaApi = new URL(this.url.api_cuaca);
    cuacaApi.search = new URLSearchParams({
      lon: longitude,
      lat: latitude
    });

    const cuacaWarningApi = new URL(this.url.api_cuaca_warning);
    cuacaWarningApi.search = new URLSearchParams({
      lat: latitude,
      long: longitude
    });
    
    const cuacaWarningHeaders = {
      'X-api-key': this.string.bmkg,
      ...this.baseHeaders
    };

    const allRequest = [
      this.mintaJson(`cuaca`, cuacaApi.toString(), { headers: this.baseHeaders }),
      this.mintaJson(`cuaca warning`, cuacaWarningApi.toString(), { headers: cuacaWarningHeaders })
    ];
    
    const [cuacaJson, cuacaWarningJson] = await Promise.all(allRequest);

    const { provinsi, kotkab, kecamatan, desa, adm4 } = cuacaJson.data.lokasi;
    const lokasi = `${desa}, ${kecamatan}, ${kotkab}, ${provinsi}`;

    const { weather, weather_desc, weather_desc_en, image, datetime, local_datetime, t, tcc, wd_deg, wd, wd_to, ws, hu, vs, vs_text } = cuacaJson.data.cuaca;
    const arahAngin = { N: 'utara', NE: "timur laut", E: 'timur', SE: 'tenggara', S: 'selatan', SW: 'barat daya', W: 'barat', NW: 'barat laut' };
    const angin = `angin bertiup dari ${arahAngin[wd]} ke ${arahAngin[wd_to]} dengan kecepatan ${ws} km/jam. sudut arah ${wd_deg}°`;
    
    const cuaca = "📍 lokasi: ".padEnd(padEnd) + lokasi + "\n" +
      "🕒 waktu: ".padEnd(padEnd) + local_datetime.split(" ")[1] + " (waktu setempat)\n" +
      "🌄 cuaca: ".padEnd(padEnd) + weather_desc + "/" + weather_desc_en + "\n" +
      "🔥 suhu: ".padEnd(padEnd) + t + "°C\n" +
      "💧 kelembapan: ".padEnd(padEnd) + hu + "%\n" +
      "🌁 tutupan awan: ".padEnd(padEnd) + tcc + "%\n" +
      "👓 jarak pandang: ".padEnd(padEnd) + vs_text + " (" + vs + " m)" + "\n" +
      "💨 angin: ".padEnd(padEnd) + angin;

    let dampak = cuacaWarningJson.data?.today?.kategoridampak;
    const peringatan = cuacaWarningJson.data?.today?.description?.description?.trim() || `(tidak ada)`;
    dampak = dampak ? JSON.parse(dampak.replaceAll(`'`, `"`))?.join(", ") : `(tidak ada)`;
    const cuacaWarning = "😱 dampak: ".padEnd(padEnd) + dampak + "\n" +
      "🚨 peringatan: ".padEnd(padEnd) + peringatan;

    const bmkgUrl = "🔗 bmkg: ".padEnd(padEnd) + `https://www.bmkg.go.id/cuaca/prakiraan-cuaca/${adm4}`;
    const gmapUrl = "🔗 google map: ".padEnd(padEnd) + `https://www.google.com/maps?q=${latitude},${longitude}`;

    const final = namaTempat + cuaca + '\n\n' + cuacaWarning + '\n\n' + bmkgUrl + '\n' + gmapUrl;
    return final;
  },

  run: async function (lokasiKamu) {
    const wolep = await this.cariKoordinat(lokasiKamu);
    const { latitude, longitude, placeName } = wolep;
    const result = await this.getkWeatherByCoordinateBMKG(latitude, longitude, placeName);
    return result;
  }
};

function cuacaMenu() {
  console.clear()
  console.log("\n🌦️ INFO CUACA BMKG");
  console.log("1. 🔍 Cari Info Cuaca");
  console.log("2. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
    console.clear()
      rl.question("\n🔍 Masukkan lokasi (contoh: Jakarta, Monas, dll): ", async (query) => {
        if (!query) return cuacaMenu();

        try {
          console.log("🔍 Mencari informasi cuaca...");
          const result = await cuaca.run(query);
          console.log("\n" + result);
          
          const filename = `cuaca_${sanitizeFilename(query)}.txt`;
          const filePath = path.join(OUTPUT_DIR, filename);
          fs.writeFileSync(filePath, result);
          console.log(`\n✅ Hasil disimpan di: ${filePath}`);
        } catch (error) {
          console.error("❌ Error:", error.message);
        }
        cuacaMenu();
      });
    } else if (choice === "2") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      cuacaMenu();
    }
  });
}

// =================== KRL Commuter Functions ===================
const KRL_API_URL = 'https://api-partner.krl.co.id';
const KRL_API_TOKEN = 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdWQiOiIzIiwianRpIjoiYmE0Yzc4MzE4ODNjYTI0N2YzMTBkMTJhYzc3ZjE5ZTdjMTVkNjgxOTk2ODM0MDc0MGM3MzliYmRjNGQ3YTI5MzczYzMyNWM2NDFiZjgxYzciLCJpYXQiOjE3NTQ0NTkxMjYsIm5iZiI6MTc1NDQ1OTEyNiwiZXhwIjoxNzg1OTk1MTI2LCJzdWIiOiI1Iiwic2NvcGVzIjpbXX0.zPA0IDAN3NycMKa6DaOdRmkcFz1oUTX1dkxEp3MLBlhibTQI0L0WB9mY-pUlQW5vQj8ktOdo-rRvrjxiXaHFqLQM6ebONbqTg8V0AjBXwrkBjLZDCE4dop9iZyDXcG2b9XTLCgPgpOBbduW_Dy0-bIkJOOIgIzl9mEEUVQf3T6G_zA796SGJ6rtLqfBK-sMnhOV4eZSqQIXIrxPyCJ8SA893p-29PFxfQfcbXW_6cYBFhDzyiilhJ6xQd6znN2eWOL4MPAxYeS2ZGnaZ7ijUN91MAyPnV0dQU7loVtS1jt2HlM5oMSsE2Zoz6FP31GvG6f7o_MWogEp0ZMOus50bVly3II8Rjjc4IGgswbw0h-RS0Ipo3f2QmXp4GfhRNUoTyqq-7oiCIDPUJcdg39lSIy9Fz7-ECNfbjEiH60V3GyftuiFGrayMoE7XeWaC9wQZo3fLHhI1aPgbXXsP-rqWLFf2km4zdG5Y5CYpUNb_Z11VOU6aaFCdRtoC6e7VcxHxLwCBT22wluNpbfFtEQSYDQE1JlegijvFmnRHTM88n-zp7sWhuCWVX6oE0ULdy51SR4iOqpYOA4B1ZymmYrQz1kBxSA_52lnTBlU9gfWkUiFX8GLSh7wQ8a4dVMYoJj6t1VCJt9-d30jn4S3tXsim_3wpp71RE9SSazV35j8o7do';

const KRL_HEADERS = {
  'Authorization': KRL_API_TOKEN,
  'Accept': 'application/json',
  'User-Agent': 'Mozilla/5.0',
  'Accept-Encoding': 'gzip, deflate',
  'Accept-Language': 'id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7'
};

async function getAllStations() {
  try {
    const response = await axios.get(`${KRL_API_URL}/krl-webs/v1/krl-station`, { headers: KRL_HEADERS });
    return response.data.data || [];
  } catch (error) {
    console.error("❌ Gagal mendapatkan daftar stasiun:", error.response?.data?.message || error.message);
    return [];
  }
}

async function getStationId(stationName) {
  if (!stationName) {
    console.log("⚠️ Nama stasiun tidak boleh kosong");
    return null;
  }

  const stations = await getAllStations();
  return stations.find(station => 
    station.sta_name.toLowerCase().includes(stationName.toLowerCase())
  );
}

async function tarifKereta(origin, destination) {
  try {
    const [originData, destinationData] = await Promise.all([
      getStationId(origin),
      getStationId(destination)
    ]);

    if (!originData || !destinationData) {
      console.log("❌ Stasiun tidak ditemukan. Pastikan nama stasiun benar.");
      return;
    }

    const response = await axios.get(
      `${KRL_API_URL}/krl-webs/v1/fare?stationfrom=${originData.sta_id}&stationto=${destinationData.sta_id}`,
      { headers: KRL_HEADERS }
    );

    const fareData = response.data.data?.[0];
    if (fareData) {
      console.log(`💰 Tarif ${originData.sta_name} → ${destinationData.sta_name}:`);
      console.log(`   - Harga: Rp ${fareData.fare}`);
      console.log(`   - Jarak: ${fareData.distance} km`);
    } else {
      console.log("ℹ️ Tarif tidak tersedia untuk rute ini");
    }
  } catch (error) {
    console.error("❌ Gagal mengambil tarif:", error.response?.data?.message || error.message);
  }
}

async function jadwalKereta(stationName, startTime, endTime) {
  try {
    const stationData = await getStationId(stationName);
    
    if (!stationData) {
      console.log("❌ Stasiun tidak ditemukan");
      return;
    }

    const response = await axios.get(
      `${KRL_API_URL}/krl-webs/v1/schedule?stationid=${stationData.sta_id}&timefrom=${startTime}&timeto=${endTime}`,
      { headers: KRL_HEADERS }
    );

    const schedules = response.data.data;
    if (schedules?.length) {
      console.log(`🚉 Jadwal KRL di ${stationData.sta_name} (${startTime} - ${endTime}):`);
      console.log("--------------------------------------------------");
      
      schedules.forEach((schedule, index) => {
        console.log(`${index + 1}. ${schedule.ka_name} → ${schedule.dest}`);
        console.log(`   🕒 Berangkat: ${schedule.time_est}`);
        console.log(`   🕒 Tiba: ${schedule.dest_time}`);
        console.log("--------------------------------------------------");
      });
    } else {
      console.log("ℹ️ Tidak ada jadwal kereta pada waktu yang diminta");
    }
  } catch (error) {
    console.error("❌ Gagal mengambil jadwal:", error.response?.data?.message || error.message);
  }
}

function krlMenu() {
  console.clear()
  console.log("\n🚉 KRL COMMUTER INFO");
  console.log("1. 🔍 Cari Tarif Perjalanan");
  console.log("2. 🕒 Cek Jadwal Kereta");
  console.log("3. 📋 Daftar Stasiun KRL");
  console.log("4. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      rl.question("\n🚉 Masukkan stasiun asal: ", (origin) => {
        rl.question("🚉 Masukkan stasiun tujuan: ", (destination) => {
          if (!origin || !destination) return krlMenu();
          tarifKereta(origin, destination);
          krlMenu();
        });
      });
    } else if (choice === "2") {
      rl.question("\n🚉 Masukkan stasiun: ", (station) => {
        rl.question("🕒 Masukkan waktu mulai (HH:MM): ", (startTime) => {
          rl.question("🕒 Masukkan waktu selesai (HH:MM): ", (endTime) => {
            if (!station || !startTime || !endTime) return krlMenu();
            jadwalKereta(station, startTime, endTime);
            krlMenu();
          });
        });
      });
    } else if (choice === "3") {
      listStations();
    } else if (choice === "4") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      krlMenu();
    }
  });
}

async function listStations() {
  try {
    console.log("\n📋 Mendapatkan daftar stasiun KRL...");
    const stations = await getAllStations();
    
    if (!stations.length) {
      console.log("❌ Gagal mendapatkan daftar stasiun");
      return krlMenu();
    }

    // Filter hanya stasiun yang aktif (fg_enable = 1)
    const activeStations = stations.filter(station => station.fg_enable === 1);
    console.clear()
    console.log("\n🚉 DAFTAR STASIUN KRL COMMUTER:");
    console.log("=".repeat(60));
    
    // Kelompokkan stasiun berdasarkan wilayah
    const groupedStations = {};
    activeStations.forEach(station => {
      const area = station.group_wil === 6 ? "Yogyakarta" : 
                  station.group_wil === 1 ? "Merak" : "Jabodetabek";
      
      if (!groupedStations[area]) {
        groupedStations[area] = [];
      }
      groupedStations[area].push(station);
    });

    // Fungsi untuk format stasiun dengan padding yang konsisten
    const formatStation = (st) => {
      const id = st.sta_id.padEnd(4, ' ');
      const name = st.sta_name.padEnd(20, ' ');
      return `${id}: ${name}`;
    };

    // Tampilkan stasiun per wilayah
    for (const [area, areaStations] of Object.entries(groupedStations)) {
      console.log(`\n🌍 AREA ${area.toUpperCase()}:`);
      console.log("-".repeat(60));
      
      // Tampilkan 3 stasiun per baris dengan format rapi
      for (let i = 0; i < areaStations.length; i += 3) {
        const lineStations = areaStations.slice(i, i + 3);
        const line = lineStations.map(st => formatStation(st)).join(" | ");
        console.log(line);
      }
    }

    console.log("\nℹ️ Total stasiun aktif: " + activeStations.length);
    
    // Tawarkan untuk menyimpan daftar stasiun
    rl.question("\n💾 Simpan daftar stasiun ke file? (y/n): ", async (answer) => {
      if (answer.toLowerCase() === 'y') {
        const filename = `daftar_stasiun_krl.txt`;
        const filePath = path.join(OUTPUT_DIR, filename);
        
        // Format untuk disimpan ke file
        let fileContent = "DAFTAR STASIUN KRL COMMUTER\n";
        fileContent += "=".repeat(60) + "\n\n";
        
        for (const [area, areaStations] of Object.entries(groupedStations)) {
          fileContent += `AREA ${area.toUpperCase()}:\n`;
          fileContent += "-".repeat(60) + "\n";
          
          for (let i = 0; i < areaStations.length; i += 3) {
            const lineStations = areaStations.slice(i, i + 3);
            const line = lineStations.map(st => formatStation(st)).join(" | ");
            fileContent += line + "\n";
          }
          fileContent += "\n";
        }
        
        fileContent += `Total stasiun aktif: ${activeStations.length}\n`;
        
        fs.writeFileSync(filePath, fileContent);
        console.log(`\n✅ Daftar stasiun disimpan di: ${filePath}`);
      }
      krlMenu();
    });
  } catch (error) {
    console.error("❌ Error:", error.message);
    krlMenu();
  }
}

// =================== GCam Support Functions ===================
const gcamator = {
  api: {
    base: 'https://gcamator.gtgroup.dev/api/values',
    endpoints: {
      jikem: '/getgcamsnew/',
      specs: (manufacturer, model) => `/getspecs/${manufacturer}/${model}`
    }
  },

  headers: {
    'user-agent': 'NB Android/1.0.0'
  },

  info: async (manufacturer, model) => {
    try {
      if (!manufacturer?.trim() || !model?.trim()) {
        return {
          success: false,
          code: 400,
          result: { error: 'Manufacturer ama modelnya wajib diisi yak bree... 🗿' }
        };
      }

      const base = `${gcamator.api.base}${gcamator.api.endpoints.jikem}`;
      const jikem = await axios.get(base, { headers: gcamator.headers });
      const gcam = jikem.data.find(
        item =>
          item.manufacturer.toLowerCase() === manufacturer.toLowerCase() &&
          item.model.toLowerCase() === model.toLowerCase()
      );

      if (!gcam) {
        return {
          success: false,
          code: 404,
          result: { error: 'Device lu kagak ada bree di databasenya Gcamator 😂' }
        };
      }

      const a = `${gcamator.api.base}${gcamator.api.endpoints.specs(manufacturer, model)}`;
      const { data: specs } = await axios.get(a, { headers: gcamator.headers });

      if (!specs?.id) {
        return {
          success: false,
          code: 404,
          result: { error: 'Spesifikasi device lu kagak ada bree.. ganti device yang lain aja dah 😂' }
        };
      }

      return {
        success: true,
        code: 200,
        result: {
          id: gcam.id,
          model: gcam.model,
          manufacturer: gcam.manufacturer,
          androidVersion: gcam.androidVersion || specs.androidVersion || '',
          appVersion: gcam.appVersion || '',
          isTested: gcam.isTested ?? false,
          download: `https://gcamator.gtgroup.dev/api/values/${gcam.id}`,
          specs: {
            id: specs.id,
            chipset: specs.chipset,
            battery: specs.battery,
            displaySize: specs.displaySize,
            displayType: specs.displayType,
            displayResolution: specs.displayResolution,
            cpu: specs.cpu,
            gpu: specs.gpu,
            internal: specs.internal,
            mainCameraSpecs: specs.mainCameraSpecs,
            selfieCameraSpecs: specs.selfieCameraSpecs,
            sensors: specs.sensors,
            colors: specs.colors,
            charging: specs.charging
          }
        }
      };
    } catch (error) {
      return {
        success: false,
        code: error?.response?.status || 500,
        result: { error: error.message || 'Error bree 😂' }
      };
    }
  }
};

function gcamMenu() {
  console.log("\n📱 GCAM SUPPORT CHECK");
  console.log("1. 🔍 Cek Dukungan GCam");
  console.log("2. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      rl.question("\n📱 Masukkan merek ponsel (contoh: Xiaomi, Samsung): ", (manufacturer) => {
        rl.question("📱 Masukkan model ponsel (contoh: Redmi Note 10, Galaxy S21): ", async (model) => {
          if (!manufacturer || !model) return gcamMenu();

          try {
            console.log("🔍 Mencari informasi GCam...");
            const result = await gcamator.info(manufacturer.trim(), model.trim());
            
            if (!result.success) {
              console.log(`❌ ${result.result.error}`);
              return gcamMenu();
            }

            const data = result.result;
            console.log("\n📱 INFORMASI GCAM:");
            console.log("=".repeat(60));
            console.log(`📱 Device: ${data.manufacturer} ${data.model}`);
            console.log(`🤖 Android: ${data.androidVersion}`);
            console.log(`📷 Versi GCam: ${data.appVersion}`);
            console.log(`🧪 Diuji: ${data.isTested ? '✅ Ya' : '❌ Tidak'}`);
            console.log(`🔗 Download: ${data.download}`);
            
            console.log("\n💻 SPESIFIKASI:");
            console.log(`🖥️ Chipset: ${data.specs.chipset}`);
            console.log(`🔋 Baterai: ${data.specs.battery}`);
            console.log(`📺 Layar: ${data.specs.displaySize} ${data.specs.displayType} (${data.specs.displayResolution})`);
            console.log(`⚙️ CPU: ${data.specs.cpu}`);
            console.log(`🎮 GPU: ${data.specs.gpu}`);
            console.log(`💾 Penyimpanan: ${data.specs.internal}`);
            console.log(`📷 Kamera Utama: ${data.specs.mainCameraSpecs}`);
            console.log(`🤳 Kamera Depan: ${data.specs.selfieCameraSpecs}`);
            console.log(`🔌 Charging: ${data.specs.charging}`);
            
            const filename = `gcam_${sanitizeFilename(manufacturer)}_${sanitizeFilename(model)}.txt`;
            const filePath = path.join(OUTPUT_DIR, filename);
            fs.writeFileSync(filePath, JSON.stringify(data, null, 2));
            console.log(`\n✅ Hasil disimpan di: ${filePath}`);
          } catch (error) {
            console.error("❌ Error:", error.message);
          }
          gcamMenu();
        });
      });
    } else if (choice === "2") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      gcamMenu();
    }
  });
}

// =================== AI Music Generator Functions ===================
const sonu = {
  api: {
    base: 'https://musicai.apihub.today/api/v1',
    endpoints: {
      register: '/users',
      create: '/song/create',
      checkStatus: '/song/user'
    }
  },

  headers: {
    'user-agent': 'NB Android/1.0.0',
    'content-type': 'application/json',
    'accept': 'application/json',
    'x-platform': 'android',
    'x-app-version': '1.0.0',
    'x-country': 'ID',
    'accept-language': 'id-ID',
    'x-client-timezone': 'Asia/Jakarta'
  },

  deviceId: require('uuid').v4(),
  userId: null,
  fcmToken: 'eqnTqlxMTSKQL5NQz6r5aP:APA91bHa3CvL5Nlcqx2yzqTDAeqxm_L_vIYxXqehkgmTsCXrV29eAak6_jqXv5v1mQrdw4BGMLXl_BFNrJ67Em0vmdr3hQPVAYF8kR7RDtTRHQ08F3jLRRI',

  register: async () => {
    const msgId = require('uuid').v4();
    const time = Date.now().toString();
    const header = {
      ...sonu.headers,
      'x-device-id': sonu.deviceId,
      'x-request-id': msgId,
      'x-message-id': msgId,
      'x-request-time': time
    };

    try {
      const response = await axios.put(
        `${sonu.api.base}${sonu.api.endpoints.register}`,
        {
          deviceId: sonu.deviceId,
          fcmToken: sonu.fcmToken
        },
        { headers: header }
      );
      sonu.userId = response.data.id;
      return {
        success: true,
        code: 200,
        result: { userId: sonu.userId }
      };
    } catch (err) {
      return {
        success: false,
        code: err.response?.status || 500,
        result: { error: err.message }
      };
    }
  },

  create: async ({ title, mood, genre, lyrics, gender }) => {
    if (!title || title.trim() === '') {
      return {
        success: false,
        code: 400,
        result: { error: "Judul lagunya kagak boleh kosong bree 😂" }
      };
    }
    if (!lyrics || lyrics.trim() === '') {
      return {
        success: false,
        code: 400,
        result: { error: "Lirik lagunya mana? Mau generate lagu kan? Yaa mana liriknya 😂" }
      };
    }
    if (lyrics.length > 1500) {
      return {
        success: false,
        code: 400,
        result: { error: "Lirik lagu kagak boleh lebih dari 1500 karakter yak bree 🗿"}
      };
    }

    const msgId = require('uuid').v4();
    const time = Date.now().toString();
    const header = {
      ...sonu.headers,
      'x-device-id': sonu.deviceId,
      'x-client-id': sonu.userId,
      'x-request-id': msgId,
      'x-message-id': msgId,
      'x-request-time': time
    };

    const body = {
      type: 'lyrics',
      name: title,
      lyrics
    };
    if (mood && mood.trim() !== '') body.mood = mood;
    if (genre && genre.trim() !== '') body.genre = genre;
    if (gender && gender.trim() !== '') body.gender = gender;

    try {
      const response = await axios.post(
        `${sonu.api.base}${sonu.api.endpoints.create}`,
        body,
        { headers: header }
      );

      return {
        success: true,
        code: 200,
        result: { songId: response.data.id }
      };
    } catch (err) {
      return {
        success: false,
        code: err.response?.status || 500,
        result: { error: err.message }
      };
    }
  },

  task: async (songId) => {
    const header = {
      ...sonu.headers,
      'x-client-id': sonu.userId
    };

    const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

    try {
      let attempt = 0;
      let found = null;

      while (true) {
        const response = await axios.get(
          `${sonu.api.base}${sonu.api.endpoints.checkStatus}`,
          {
            params: {
              userId: sonu.userId,
              isFavorite: false,
              page: 1,
              searchText: ''
            },
            headers: header
          }
        );

        found = response.data.datas.find(song => song.id === songId);
        if (!found) {
          return {
            success: false,
            code: 404,
            result: { error: "Lagunya belum jadi keknya bree, soalnya kagak ada 😂" }
          };
        }

        process.stdout.write(`\r🔄 [${++attempt}] Status: ${found.status} | Proses: ${found.url ? '✅ Done' : '⏳ Loading...'}`);

        if (found.url) {
          return {
            success: true,
            code: 200,
            result: {
              status: found.status,
              songId: found.id,
              title: found.name,
              username: found.username,
              url: found.url,
              thumbnail: found.thumbnail_url
            }
          };
        }

        await delay(3000);
      }
    } catch (err) {
      return {
        success: false,
        code: err.response?.status || 500,
        result: { error: err.message }
      };
    }
  }
};

function aiMusicMenu() {
  console.log("\n🎵 AI MUSIC GENERATOR");
  console.log("1. 🎼 Buat Lagu AI");
  console.log("2. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      rl.question("\n🎼 Masukkan judul lagu: ", (title) => {
        rl.question("🎵 Masukkan lirik lagu: ", (lyrics) => {
          rl.question("😊 Mood lagu (opsional, contoh: happy, sad): ", (mood) => {
            rl.question("🎶 Genre lagu (opsional, contoh: pop, rock): ", (genre) => {
              rl.question("👤 Gender vokal (opsional, male/female): ", async (gender) => {
                if (!title || !lyrics) {
                  console.log("❌ Judul dan lirik wajib diisi");
                  return aiMusicMenu();
                }

                try {
                  console.log("\n🔍 Mendaftarkan perangkat...");
                  const regResult = await sonu.register();
                  if (!regResult.success) {
                    console.log(`❌ Gagal mendaftar: ${regResult.result.error}`);
                    return aiMusicMenu();
                  }

                  console.log("\n🎵 Membuat lagu AI...");
                  const createResult = await sonu.create({
                    title: title.trim(),
                    lyrics: lyrics.trim(),
                    mood: mood?.trim(),
                    genre: genre?.trim(),
                    gender: gender?.trim()
                  });

                  if (!createResult.success) {
                    console.log(`❌ Gagal membuat lagu: ${createResult.result.error}`);
                    return aiMusicMenu();
                  }

                  console.log("\n⏳ Memproses lagu (ini mungkin butuh beberapa menit)...");
                  const taskResult = await sonu.task(createResult.result.songId);
                  
                  if (!taskResult.success) {
                    console.log(`\n❌ Gagal memproses lagu: ${taskResult.result.error}`);
                    return aiMusicMenu();
                  }

                  const song = taskResult.result;
                  console.log("\n\n🎶 LAGU BERHASIL DIBUAT!");
                  console.log("=".repeat(60));
                  console.log(`🎵 Judul: ${song.title}`);
                  console.log(`👤 Pembuat: ${song.username}`);
                  console.log(`📊 Status: ${song.status}`);
                  console.log(`🔗 URL: ${song.url}`);
                  console.log(`🖼️ Thumbnail: ${song.thumbnail}`);

                  const filename = `ai_music_${sanitizeFilename(title)}.txt`;
                  const filePath = path.join(OUTPUT_DIR, filename);
                  fs.writeFileSync(filePath, JSON.stringify(song, null, 2));
                  console.log(`\n✅ Hasil disimpan di: ${filePath}`);
                } catch (error) {
                  console.error("❌ Error:", error.message);
                }
                aiMusicMenu();
              });
            });
          });
        });
      });
    } else if (choice === "2") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      aiMusicMenu();
    }
  });
}

// =================== Multi-Platform Downloader (Dlpanda) Functions ===================
class Dlpanda {
  origin = "https://dlpanda.com/id";
  webHeaders = {
    "accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
    "accept-encoding": "gzip, deflate, br, zstd",
    "accept-language": "en-GB,en;q=0.9,en-US;q=0.8",
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "priority": "u=0, i",
    "sec-ch-ua": "\"Not)A;Brand\";v=\"8\", \"Chromium\";v=\"138\", \"Microsoft Edge\";v=\"138\"",
    "sec-ch-ua-mobile": "?0",
    "sec-ch-ua-platform": "\"Windows\"",
    "sec-fetch-dest": "document",
    "sec-fetch-mode": "navigate",
    "sec-fetch-site": "none",
    "sec-fetch-user": "?1",
    "upgrade-insecure-requests": "1",
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/138.0.0.0 Safari/537.36 Edg/138.0.0.0"
  };

  async getHtml(description, api, opts) {
    try {
      const response = await axios({
        url: api,
        method: opts.method || 'GET',
        data: opts.body,
        headers: opts.headers,
        responseType: 'text'
      });
      if (response.status < 200 || response.status >= 300) {
        throw new Error(`${response.status} ${response.statusText}\n${response.data}`);
      }
      return response.data;
    } catch (err) {
      throw new Error(`gagal getHtml ${description} karena ${err.message}`);
    }
  }

  async getCookieAndToken(path, regex) {
    try {
      const url = this.origin + path;
      const response = await axios.get(url, {
        headers: this.webHeaders
      });
      
      const cookie = response.headers['set-cookie']?.map(v => v.split(";")[0]).join("; ") || '';
      const html = response.data;
      const token = html.match(new RegExp(regex))?.[1] || null;
      return { cookie, token };
    } catch (err) {
      throw new Error(`gagal mendapatkan kuki dan tokenF\nkarena ${err.message}`);
    }
  }

  async facebook(url) {
    if (typeof (url) !== "string" || url?.trim()?.length === 0) throw new Error(`miaw.. masukin url yang bener yah`);
    const { cookie, token } = await this.getCookieAndToken(`/facebook`, `_token" value="(.+?)"`);

    const headers = { cookie, ...this.webHeaders };
    const body = new URLSearchParams({ url, _token: token });

    const api = new URL(this.origin);
    api.pathname = "/id/facebook";

    const html = await this.getHtml(`facebook`, api.toString(), { headers, data: body.toString(), method: "post" });

    // pick link
    const text = html.match(/" target="_blank"><h5>(.+?)<\/h5>/)?.[1] || `from facebook`;
    let images = Array.from(html.matchAll(/img alt="" src="(.+?)"/gm));
    if (images.length) images = images.map(v => v[1]);
    const audio = html.match(/downVideo\('([^ ]+)', '(?:.+?)mp3/)?.[1] || null;
    let video = html.match(/<source src="(.+?)"/)?.[1] || null;
    if (video) video = video.replaceAll(`&amp;`,`&`);
    return {text, audio, video, images};             
  }

  async twitter(url) {
    if (typeof (url) !== "string" || url?.trim()?.length === 0) throw new Error(`miaw.. masukin url yang bener yah`);
    const { cookie, token } = await this.getCookieAndToken(`/t`, `_token" value="(.+?)"`);
    const headers = { cookie, ...this.webHeaders };
    const body = new URLSearchParams({ url, _token: token });

    const api = new URL(this.origin);
    api.pathname = "/id/t";

    const html = await this.getHtml(`twitter`, api.toString(), {headers, data: body.toString(), method: "post"});

    //pick link
    const text = html.match(/" target="_blank"><h5>(.+?)<\/h5>/)?.[1] || `from Pinterest`;
    let images = Array.from(html.matchAll(/img alt="" src="(.+?)"/gm));
    if (images.length) images = images.map(v => v[1]);
    const audio = html.match(/downVideo\('([^ ]+)', '(?:.+?)mp3/)?.[1] || null;
    const video = html.match(/<source src="(.+?)"/)?.[1] || null;
    return {text, audio, video, images};
  }

  async tiktok(url) {
    if (typeof (url) !== "string" || url?.trim()?.length === 0) throw new Error(`miaw.. masukin url yang bener yah`);
    const { cookie, token } = await this.getCookieAndToken(``, `id="token" value="(.+?)"`);
    const headers = { cookie, ...this.webHeaders };

    const api = new URL(this.origin);
    api.search = new URLSearchParams({ url, token });

    const html = await this.getHtml(`tiktok`, api.toString(), {headers});

    // pick link
    const text = html.match(/" target="_blank"><h5>(.+?)<\/h5>/)?.[1] || `from Tiktok`;
    let images = Array.from(html.matchAll(/img alt="" src="(.+?)"/gm));
    if (images.length) images = images.map(v => v[1]);
    const audio = html.match(/downVideo\('([^ ]+)', '(?:.+?)mp3/)?.[1] || null;
    let video = html.match(/downVideo\('([^ ]+)', '(?:.+?)mp4/)?.[1] || null;
    if (video) video = "https:" + video;

    return {text, audio, video, images};
  }

  async pinterest(url) {
    if (typeof (url) !== "string" || url?.trim()?.length === 0) throw new Error(`miaw.. masukin url yang bener yah`);    

    const headers = {...this.webHeaders};
    const api = new URL(this.origin);
    api.search = new URLSearchParams({ url });
    api.pathname = "id/pinterest";

    const html = await this.getHtml(`pinterst`, api.toString(), {headers});

    // pick link
    let text = html.match(/" target="_blank"><h5>(.+?)<\/h5>/)?.[1] || `from Pinterest`;
    let images = Array.from(html.matchAll(/img alt="" src="(.+?)"/gm));
    if (images.length) images = images.map(v => v[1]);
    const audio = html.match(/downVideo\('([^ ]+)', '(?:.+?)mp3/)?.[1] || null;
    let video = html.match(/downloadFileHref\('(.+?)'\)/)?.[1] || null;
    return {text, audio, video, images};
  }
}

function dlpandaMenu() {
  console.log("\n📥 MULTI-PLATFORM DOWNLOADER");
  console.log("1. 📱 Download dari Facebook");
  console.log("2. 🐦 Download dari Twitter/X");
  console.log("3. 🎵 Download dari TikTok");
  console.log("4. 📌 Download dari Pinterest");
  console.log("5. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      rl.question("\n🔗 Masukkan URL Facebook: ", async (url) => {
        if (!url) return dlpandaMenu();
        
        try {
          console.log("🔍 Memproses URL Facebook...");
          const dlpanda = new Dlpanda();
          const result = await dlpanda.facebook(url);
          
          console.log("\n📝 Informasi Media:");
          console.log("=".repeat(60));
          console.log(`📌 Deskripsi: ${result.text}`);
          
          if (result.video) {
            console.log("\n🎥 Video ditemukan:");
            console.log(result.video);
            
            rl.question("\n💾 Download video? (y/n): ", async (answer) => {
              if (answer.toLowerCase() === 'y') {
                const filename = `facebook_${Date.now()}.mp4`;
                const filePath = path.join(OUTPUT_DIR, filename);
                await downloadFile(result.video, filePath);
                console.log(`\n✅ Video disimpan di: ${filePath}`);
              }
              dlpandaMenu();
            });
          } else if (result.images.length) {
            console.log("\n🖼️ Gambar ditemukan:");
            result.images.forEach((img, i) => console.log(`${i + 1}. ${img}`));
            
            rl.question("\n💾 Download semua gambar? (y/n): ", async (answer) => {
              if (answer.toLowerCase() === 'y') {
                for (let i = 0; i < result.images.length; i++) {
                  const imgUrl = result.images[i];
                  const filename = `facebook_${Date.now()}_${i + 1}.jpg`;
                  const filePath = path.join(OUTPUT_DIR, filename);
                  await downloadFile(imgUrl, filePath);
                  console.log(`✅ Gambar ${i + 1} disimpan di: ${filePath}`);
                }
              }
              dlpandaMenu();
            });
          } else {
            console.log("❌ Tidak ada media yang ditemukan");
            dlpandaMenu();
          }
        } catch (error) {
          console.error("❌ Error:", error.message);
          dlpandaMenu();
        }
      });
    } else if (choice === "2") {
      rl.question("\n🔗 Masukkan URL Twitter/X: ", async (url) => {
        if (!url) return dlpandaMenu();
        
        try {
          console.log("🔍 Memproses URL Twitter/X...");
          const dlpanda = new Dlpanda();
          const result = await dlpanda.twitter(url);
          
          console.log("\n📝 Informasi Media:");
          console.log("=".repeat(60));
          console.log(`📌 Deskripsi: ${result.text}`);
          
          if (result.video) {
            console.log("\n🎥 Video ditemukan:");
            console.log(result.video);
            
            rl.question("\n💾 Download video? (y/n): ", async (answer) => {
              if (answer.toLowerCase() === 'y') {
                const filename = `twitter_${Date.now()}.mp4`;
                const filePath = path.join(OUTPUT_DIR, filename);
                await downloadFile(result.video, filePath);
                console.log(`\n✅ Video disimpan di: ${filePath}`);
              }
              dlpandaMenu();
            });
          } else if (result.images.length) {
            console.log("\n🖼️ Gambar ditemukan:");
            result.images.forEach((img, i) => console.log(`${i + 1}. ${img}`));
            
            rl.question("\n💾 Download semua gambar? (y/n): ", async (answer) => {
              if (answer.toLowerCase() === 'y') {
                for (let i = 0; i < result.images.length; i++) {
                  const imgUrl = result.images[i];
                  const filename = `twitter_${Date.now()}_${i + 1}.jpg`;
                  const filePath = path.join(OUTPUT_DIR, filename);
                  await downloadFile(imgUrl, filePath);
                  console.log(`✅ Gambar ${i + 1} disimpan di: ${filePath}`);
                }
              }
              dlpandaMenu();
            });
          } else {
            console.log("❌ Tidak ada media yang ditemukan");
            dlpandaMenu();
          }
        } catch (error) {
          console.error("❌ Error:", error.message);
          dlpandaMenu();
        }
      });
    } else if (choice === "3") {
      rl.question("\n🔗 Masukkan URL TikTok: ", async (url) => {
        if (!url) return dlpandaMenu();
        
        try {
          console.log("🔍 Memproses URL TikTok...");
          const dlpanda = new Dlpanda();
          const result = await dlpanda.tiktok(url);
          
          console.log("\n📝 Informasi Media:");
          console.log("=".repeat(60));
          console.log(`📌 Deskripsi: ${result.text}`);
          
          if (result.video) {
            console.log("\n🎥 Video ditemukan:");
            console.log(result.video);
            
            rl.question("\n💾 Download video? (y/n): ", async (answer) => {
              if (answer.toLowerCase() === 'y') {
                const filename = `tiktok_${Date.now()}.mp4`;
                const filePath = path.join(OUTPUT_DIR, filename);
                await downloadFile(result.video, filePath);
                console.log(`\n✅ Video disimpan di: ${filePath}`);
              }
              dlpandaMenu();
            });
          } else if (result.images.length) {
            console.log("\n🖼️ Gambar ditemukan:");
            result.images.forEach((img, i) => console.log(`${i + 1}. ${img}`));
            
            rl.question("\n💾 Download semua gambar? (y/n): ", async (answer) => {
              if (answer.toLowerCase() === 'y') {
                for (let i = 0; i < result.images.length; i++) {
                  const imgUrl = result.images[i];
                  const filename = `tiktok_${Date.now()}_${i + 1}.jpg`;
                  const filePath = path.join(OUTPUT_DIR, filename);
                  await downloadFile(imgUrl, filePath);
                  console.log(`✅ Gambar ${i + 1} disimpan di: ${filePath}`);
                }
              }
              dlpandaMenu();
            });
          } else {
            console.log("❌ Tidak ada media yang ditemukan");
            dlpandaMenu();
          }
        } catch (error) {
          console.error("❌ Error:", error.message);
          dlpandaMenu();
        }
      });
    } else if (choice === "4") {
      rl.question("\n🔗 Masukkan URL Pinterest: ", async (url) => {
        if (!url) return dlpandaMenu();
        
        try {
          console.log("🔍 Memproses URL Pinterest...");
          const dlpanda = new Dlpanda();
          const result = await dlpanda.pinterest(url);
          
          console.log("\n📝 Informasi Media:");
          console.log("=".repeat(60));
          console.log(`📌 Deskripsi: ${result.text}`);
          
          if (result.video) {
            console.log("\n🎥 Video ditemukan:");
            console.log(result.video);
            
            rl.question("\n💾 Download video? (y/n): ", async (answer) => {
              if (answer.toLowerCase() === 'y') {
                const filename = `pinterest_${Date.now()}.mp4`;
                const filePath = path.join(OUTPUT_DIR, filename);
                await downloadFile(result.video, filePath);
                console.log(`\n✅ Video disimpan di: ${filePath}`);
              }
              dlpandaMenu();
            });
          } else if (result.images.length) {
            console.log("\n🖼️ Gambar ditemukan:");
            result.images.forEach((img, i) => console.log(`${i + 1}. ${img}`));
            
            rl.question("\n💾 Download semua gambar? (y/n): ", async (answer) => {
              if (answer.toLowerCase() === 'y') {
                for (let i = 0; i < result.images.length; i++) {
                  const imgUrl = result.images[i];
                  const filename = `pinterest_${Date.now()}_${i + 1}.jpg`;
                  const filePath = path.join(OUTPUT_DIR, filename);
                  await downloadFile(imgUrl, filePath);
                  console.log(`✅ Gambar ${i + 1} disimpan di: ${filePath}`);
                }
              }
              dlpandaMenu();
            });
          } else {
            console.log("❌ Tidak ada media yang ditemukan");
            dlpandaMenu();
          }
        } catch (error) {
          console.error("❌ Error:", error.message);
          dlpandaMenu();
        }
      });
    } else if (choice === "5") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      dlpandaMenu();
    }
  });
}

// VirusTotal Functions
async function makeApiRequest(url, method, headers, body = null) {
  const response = await axios({
    url,
    method,
    headers,
    data: body,
    validateStatus: () => true
  });
  if (response.status < 200 || response.status >= 300) {
    throw new Error("VirusTotal API Error: " + response.status);
  }
  return response.data;
}

async function getAnalysisResults(analysisId) {
  const url = `${API_ENDPOINT_ANALYSES}/${analysisId}`;
  const headers = { "x-apikey": VT_API_KEY };
  return makeApiRequest(url, "GET", headers);
}

async function analyzeUrl(url) {
  const formData = new URLSearchParams();
  formData.append("url", url);
  const headers = {
    "x-apikey": VT_API_KEY,
    "Content-Type": "application/x-www-form-urlencoded"
  };
  const data = await makeApiRequest(API_ENDPOINT_URLS, "POST", headers, formData);
  return data.data.id;
}

async function uploadFile(fileBuffer) {
  const form = new FormData();
  form.append('file', fileBuffer, { filename: 'file' });
  const headers = {
    "x-apikey": VT_API_KEY,
    ...form.getHeaders()
  };
  const data = await makeApiRequest(API_ENDPOINT_FILES, "POST", headers, form);
  return data.data.id;
}

async function checkUrl(url) {
  const analysisId = await analyzeUrl(url);
  const pollAnalysis = async () => {
    const analysisResults = await getAnalysisResults(analysisId);
    if (analysisResults.data.attributes.status === "completed") {
      return analysisResults;
    } else {
      await new Promise((r) => setTimeout(r, 5000));
      return pollAnalysis();
    }
  };
  const results = await pollAnalysis();
  return parseUrlResult(results);
}

async function checkFile(fileBuffer) {
  const analysisId = await uploadFile(fileBuffer);
  const pollAnalysis = async () => {
    const analysisResults = await getAnalysisResults(analysisId);
    if (analysisResults.data.attributes.status === "completed") {
      return analysisResults;
    } else {
      await new Promise((r) => setTimeout(r, 5000));
      return pollAnalysis();
    }
  };
  const results = await pollAnalysis();
  return parseFileResult(results);
}

function parseUrlResult(analysisResults) {
  const stats = analysisResults.data.attributes.stats;
  const vendors = analysisResults.data.attributes.results;
  let status = "🟢 Safe";
  if (stats.malicious > 0) status = "🔴 Malicious";
  else if (stats.suspicious > 0) status = "🟡 Suspicious";
  return {
    vendors,
    message: `VIRUSTOTAL URL ANALYSIS\nStatus: ${status}\nMalicious: ${stats.malicious}\nSuspicious: ${stats.suspicious}\n`
  };
}

function parseFileResult(analysisResults) {
  const stats = analysisResults.data.attributes.stats;
  const vendors = analysisResults.data.attributes.results;
  let status = "🟢 Safe";
  if (stats.malicious > 0) status = "🔴 Malicious";
  else if (stats.suspicious > 0) status = "🟡 Suspicious";
  return {
    vendors,
    message: `VIRUSTOTAL FILE ANALYSIS\nStatus: ${status}\nMalicious: ${stats.malicious}\nSuspicious: ${stats.suspicious}\n`
  };
}

async function saveResultToFile(data, filename) {
  if (!fs.existsSync(OUTPUT_DIR)) fs.mkdirSync(OUTPUT_DIR, { recursive: true });
  const filePath = path.join(OUTPUT_DIR, filename);
  fs.writeFileSync(filePath, JSON.stringify(data, null, 2));
  console.log(`✅ Hasil tersimpan di: ${filePath}`);
}

function virusTotalMenu() {
  console.log("\n🛡️ VIRUSTOTAL SCAN");
  console.log("1. 🔗 Scan URL");
  console.log("2. 📂 Scan File (belum diimplementasikan)");
  console.log("3. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      rl.question("\n🔗 Masukkan URL untuk di-scan: ", async (url) => {
        if (!url) return virusTotalMenu();
        try {
          console.log("🔍 Memulai scan URL...");
          const result = await checkUrl(url);
          console.log(result.message);
          // Optionally save detailed vendor results
          // await saveResultToFile(result.vendors, `virustotal_url_${sanitizeFilename(url)}.json`);
        } catch (error) {
          console.error("❌ Error:", error.message);
        }
        virusTotalMenu();
      });
    } else if (choice === "2") {
      console.log("Fitur scan file belum diimplementasikan.");
      virusTotalMenu();
    } else if (choice === "3") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      virusTotalMenu();
    }
  });
}

// =================== Spotify Functions ====================
async function spotifyCreds() {
  try {
    const json = await (await axios.post('https://accounts.spotify.com/api/token', 'grant_type=client_credentials', {
      headers: { Authorization: 'Basic ' + Buffer.from(process.env.SPOTIFY_CLIENT_ID + ':' + process.env.SPOTIFY_CLIENT_SECRET).toString('base64') }
    })).data;
    if (!json.access_token) return { status: false, msg: `Can't generate token!` };
    return { status: true, data: json };
  } catch (e) {
    return { status: false, msg: e.message };
  }
}

async function searchingSpotify(query, type = 'track', limit = 20) {
  const creds = await spotifyCreds();
  if (!creds.status) return creds;
  const json = await (await axios.get(`https://api.spotify.com/v1/search?query=${encodeURIComponent(query)}&type=${type}&offset=0&limit=${limit}`, {
    headers: { Authorization: 'Bearer ' + creds.data.access_token }
  })).data;
  if (!json.tracks || !json.tracks.items || json.tracks.items.length < 1) return { status: false, msg: 'Music not found!' };
  let data = json.tracks.items.map(v => ({
    title: v.album.artists[0].name + ' - ' + v.name,
    duration: convert(v.duration_ms),
    popularity: v.popularity + '%',
    preview: v.preview_url,
    url: v.external_urls.spotify
  }));
  return { status: true, data };
}

async function spotifydl(url) {
  try {
    const yanzz = await axios.get(`https://api.fabdl.com/spotify/get?url=${encodeURIComponent(url)}`);
    const yanz = await axios.get(`https://api.fabdl.com/spotify/mp3-convert-task/${yanzz.data.result.gid}/${yanzz.data.result.id}`);
    return {
      title: yanzz.data.result.name,
      type: yanzz.data.result.type,
      artis: yanzz.data.result.artists,
      durasi: yanzz.data.result.duration_ms,
      image: yanzz.data.result.image,
      download: "https://api.fabdl.com" + yanz.data.result.download_url
    };
  } catch (error) {
    throw new Error(`Gagal download dari Spotify: ${error.message}`);
  }
}

function spotifyMenu() {
  console.log("\n🎧 SPOTIFY DOWNLOADER");
  console.log("1. 🔍 Cari Lagu Spotify");
  console.log("2. 📥 Download dari URL Spotify");
  console.log("3. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      searchSpotifyMusic();
    } else if (choice === "2") {
      downloadFromSpotifyUrl();
    } else if (choice === "3") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      spotifyMenu();
    }
  });
}

async function searchSpotifyMusic() {
  rl.question("\n🔍 Masukkan nama lagu atau artis: ", async (query) => {
    if (!query) return spotifyMenu();

    try {
      console.log(`🎵 Mencari "${query}" di Spotify...`);
      const result = await searchingSpotify(query, 'track', 10);

      if (!result.status) {
        console.log(`❌ ${result.msg}`);
        return spotifyMenu();
      }

      console.log("\n🎶 Hasil Pencarian:");
      console.log("=".repeat(60));
      result.data.forEach((track, index) => {
        console.log(`${index + 1}. ${track.title}`);
        console.log(`   ⏱ Durasi: ${track.duration} | 🔥 Popularitas: ${track.popularity}`);
        console.log(`   🔗 URL: ${track.url}`);
        console.log("-".repeat(40));
      });

      rl.question("\nPilih nomor lagu (1-10) atau 0 untuk kembali: ", async (choice) => {
        const index = parseInt(choice) - 1;
        if (choice === "0") return spotifyMenu();
        if (isNaN(index) || index < 0 || index >= result.data.length) {
          console.log("❌ Pilihan tidak valid");
          return searchSpotifyMusic();
        }

        const selectedTrack = result.data[index];
        console.log(`\n📥 Memproses download: ${selectedTrack.title}`);

        try {
          const downloadData = await spotifydl(selectedTrack.url);
          const filename = `${sanitizeFilename(downloadData.title)}_${pickRandom("spotify", "spot", "music")}.mp3`;
          const filePath = path.join(OUTPUT_DIR, filename);

          console.log("\n⬇️ Mengunduh dari Spotify...");
          await downloadFile(downloadData.download, filePath);
          console.log(`✅ Berhasil! File tersimpan di: ${filePath}`);
        } catch (error) {
          console.error(`❌ Gagal: ${error.message}`);
        }

        spotifyMenu();
      });
    } catch (error) {
      console.error("❌ Error:", error.message);
      spotifyMenu();
    }
  });
}

async function downloadFromSpotifyUrl() {
  rl.question("\n📥 Masukkan URL Spotify: ", async (url) => {
    if (!url) return spotifyMenu();

    if (!url.includes('spotify.com/track/')) {
      console.log("❌ URL tidak valid! Pastikan URL adalah link track Spotify");
      return downloadFromSpotifyUrl();
    }

    try {
      console.log("🔍 Memproses URL Spotify...");
      const downloadData = await spotifydl(url);
      console.log("\n🎵 Informasi Lagu:");
      console.log("=".repeat(40));
      console.log(`🎧 Judul: ${downloadData.title}`);
      console.log(`👤 Artis: ${downloadData.artis}`);
      console.log(`⏱ Durasi: ${convert(downloadData.durasi)}`);
      console.log("=".repeat(40));

      const filename = `${sanitizeFilename(downloadData.title)}_${pickRandom("spotify", "spot", "music")}.mp3`;
      const filePath = path.join(OUTPUT_DIR, filename);

      console.log("\n⬇️ Mengunduh dari Spotify...");
      await downloadFile(downloadData.download, filePath);
      console.log(`\n✅ Berhasil! File tersimpan di: ${filePath}`);
    } catch (error) {
      console.error("❌ Error:", error.message);
      console.log("\n💡 Tips:");
      console.log("- Pastikan URL Spotify valid");
      console.log("- Cek koneksi internet Anda");
      console.log("- Coba lagi nanti jika server sibuk");
    }

    spotifyMenu();
  });
}

// TopUp DANA
async function topupDana() {
  rl.question("\n💳 Masukkan nomor dan jumlah (contoh: 08123456789|10000): ", async (input) => {
    if (!input) return promptMenu();

    const [number, amount] = input.split('|');

    if (!number || !amount) {
      console.log("❌ Format salah! Contoh: 08123456789|10000");
      return topupDana();
    }

    try {
      console.log("\n🔍 Memproses pembayaran...");

      const response = await axios.get("https://api.neoxr.eu/api/topup-dana", {
        params: {
          number: number,
          amount: amount,
          apikey: API_KEY
        },
        timeout: 30000
      });

      if (response.data.status && response.data.data) {
        const paymentData = response.data.data;

        console.clear();
        console.log("\n✅ TOP UP DANA BERHASIL DIPROSES");
        console.log("══════════════════════════════");
        console.log(`📌 ID Transaksi: ${paymentData.id}`);
        console.log(`📱 Nomor: ${paymentData.number}`);
        console.log(`💳 Jumlah: ${paymentData.price_format}`);
        console.log(`⏰ Kadaluwarsa: ${paymentData.expired_at}`);
        console.log("══════════════════════════════")
        if (paymentData.qr_image) {
          const qrFilename = `dana_qr_${paymentData.id}.png`;
          const qrPath = path.join(OUTPUT_DIR, qrFilename);

          fs.writeFileSync(qrPath, paymentData.qr_image, 'base64');
          console.log(`📸 QR Code tersimpan di: ${qrPath}`);
        }
      } else {
        throw new Error("Gagal mendapatkan data pembayaran");
      }
    } catch (error) {
      console.error("\n❌ Error:", error.message);
      console.log("\n💡 Tips:");
      console.log("- Pastikan format input benar");
      console.log("- Cek koneksi internet Anda");
      console.log("- Coba lagi nanti jika server sibuk");
    }

    promptMenu();
  });
}

// Music menu
function musicMenu() {
  console.log("\n🎵 MUSIC DOWNLOADER");
  console.log("1. 🔍 Cari dan Download Lagu");
  console.log("2. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      searchAndDownloadMusic();
    } else if (choice === "2") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      musicMenu();
    }
  });
}

async function getTranscript(saveToFile = false) {
  rl.question("\n📝 Masukkan URL YouTube: ", async (url) => {
    if (!url) return transcriptMenu();

    try {
      console.log("🔍 Mengambil transcript...");
      const response = await axios.get("https://api.neoxr.eu/api/transcript", {
        params: { url, apikey: API_KEY },
        timeout: 30000
      });

      if (!response.data.status) throw new Error("Gagal mendapatkan transcript");

      const transcript = response.data.data.text;
      console.log("\n📄 Transcript:");
      console.log("=".repeat(60));
      console.log(transcript.substring(0, 200) + (transcript.length > 200 ? "..." : ""));
      console.log("=".repeat(60));
      console.log(`📊 Total karakter: ${transcript.length}`);

      if (saveToFile) {
        const filename = `transcript_${pickRandom("file","hasil")}.txt`;
        const filePath = path.join(OUTPUT_DIR, filename);

        fs.writeFileSync(filePath, transcript, 'utf8');
        console.log(`\n✅ Berhasil disimpan di: ${filePath}`);
      }
    } catch (error) {
      console.error("❌ Error:", error.message);
    }

    transcriptMenu();
  });
}

function transcriptMenu() {
  console.log("\n📝 YOUTUBE TRANSCRIPT");
  console.log("1. 📄 Ambil Transcript");
  console.log("2. 💾 Simpan ke File");
  console.log("3. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      getTranscript(false);
    } else if (choice === "2") {
      getTranscript(true);
    } else if (choice === "3") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      transcriptMenu();
    }
  });
}

async function searchAndDownloadMusic() {
  rl.question("\n🎵 Masukkan judul lagu: ", async (query) => {
    if (!query) return musicMenu();

    try {
      console.log(`🔍 Mencari: "${query}"...`);
      const response = await axios.get("https://api.neoxr.eu/api/play", {
        params: { q: query, apikey: API_KEY },
        timeout: 30000
      });

      if (!response.data.status) throw new Error("Lagu tidak ditemukan");

      const data = response.data;
      console.clear();
      console.log("\n📀 Informasi Lagu:");
      console.log("=".repeat(40));
      console.log(`🎵 Judul: ${data.title}`);
      console.log(`👤 Artist: ${data.channel}`);
      console.log(`⏱ Durasi: ${data.fduration}`);
      console.log("=".repeat(40));

      rl.question("\n💾 Download lagu ini? (y/n): ", async (answer) => {
        if (answer.toLowerCase() === 'y') {
          const filename = `${sanitizeFilename(data.title)}.mp3`;
          const filePath = path.join(OUTPUT_DIR, filename);

          console.log("\n⬇️ Mengunduh lagu...");
          await downloadFile(data.data.url, filePath);

          console.log(`\n✅ Berhasil! File tersimpan di: ${filePath}`);
        }
        musicMenu();
      });
    } catch (error) {
      console.error("❌ Error:", error.message);
      musicMenu();
    }
  });
}

async function downloadFile(url, filePath) {
  if (!fs.existsSync(OUTPUT_DIR)) {
    fs.mkdirSync(OUTPUT_DIR, { recursive: true });
  }

  const response = await axios({
    method: 'GET',
    url: url,
    responseType: 'stream',
    timeout: 60000
  });

  const writer = fs.createWriteStream(filePath);
  let downloadedBytes = 0;
  const totalBytes = parseInt(response.headers['content-length']) || 0;

  response.data.on('data', (chunk) => {
    downloadedBytes += chunk.length;
    const progress = totalBytes > 0
      ? ((downloadedBytes / totalBytes) * 100).toFixed(1)
      : downloadedBytes;
    process.stdout.write(`\r📊 Progress: ${progress}${totalBytes > 0 ? '%' : ' bytes'}`);
  });

  response.data.pipe(writer);

  return new Promise((resolve, reject) => {
    writer.on('finish', resolve);
    writer.on('error', reject);
  });
}

// TikTok & Instagram & existing tools:
function downloadTikTok() {
  rl.question("\nMasukkan link TikTok: ", async (url) => {
    if (!url) return promptMenu();

    try {
      console.log("🔍 Memproses link TikTok...");
      const response = await axios.get("https://api.neoxr.eu/api/tiktok", {
        params: { url, apikey: API_KEY },
        timeout: 30000
      });

      if (!response.data.status) throw new Error("Gagal mendapatkan data TikTok");

      const data = response.data.data;

      console.clear();
      console.log("\n📝 Caption:", data.caption);
      console.log("👤 Author:", data.author.nickname);

      // Jika ada foto
      if (data.photo && data.photo.length > 0) {
        console.log("\n📸 Deteksi konten foto...");
        for (let i = 0; i < data.photo.length; i++) {
          const photoUrl = data.photo[i];
          const filename = `${sanitizeFilename(data.author.nickname)}_${pickRandom("ttfoto","tiktokfoto", "ttimage","ttimg")}_${i + 1}.jpg`;
          const filePath = path.join(OUTPUT_DIR, filename);

          console.log(`⬇️ Mengunduh foto ${i + 1}...`);
          await downloadFile(photoUrl, filePath);
          console.log(`✅ Foto ${i + 1} tersimpan di: ${filePath}`);
        }
      }
      // Jika ada video
      else if (data.video || data.videoWM) {
        const videoUrl = data.video || data.videoWM;
        const filename = `${sanitizeFilename(data.author.nickname)}_${pickRandom("ttvid","ttvideo","tiktokvid","tiktokvideo")}.mp4`;
        const filePath = path.join(OUTPUT_DIR, filename);

        console.log("\n⬇️ Mengunduh video TikTok...");
        await downloadFile(videoUrl, filePath);
        console.log(`✅ Video tersimpan di: ${filePath}`);
      } else {
        throw new Error("Konten tidak dikenali (bukan video atau foto)");
      }
    } catch (error) {
      console.error("❌ Error:", error.message);
    }

    promptMenu();
  });
}

function downloadInstagram() {
  rl.question("\nMasukkan link Instagram: ", async (url) => {
    if (!url) return promptMenu();

    try {
      console.log("🔍 Memproses link Instagram...");
      const response = await axios.get("https://api.neoxr.eu/api/ig", {
        params: { url, apikey: API_KEY },
        timeout: 30000
      });

      if (!response.data.status || !response.data.data.length) {
        throw new Error("Gagal mendapatkan data Instagram");
      }

      const media = response.data.data[0];
      const filename = `${pickRandom("ig","insta","instagram","ins")}.${media.type}`;
      const filePath = path.join(OUTPUT_DIR, filename);

      console.log("\n⬇️ Mengunduh media Instagram...");
      await downloadFile(media.url, filePath);

      console.log(`\n✅ Berhasil! File tersimpan di: ${filePath}`);
    } catch (error) {
      console.error("❌ Error:", error.message);
    }

    promptMenu();
  });
}

// NGL Spam & OTP spam
function nglSpamMenu() {
  console.clear();
  console.log("\n🔥 NGL SPAM TOOL");
  rl.question("Masukkan username NGL: ", async (username) => {
    if (!username) return promptMenu();

    rl.question("Masukkan jumlah spam (max 500): ", async (jumlah) => {
      jumlah = parseInt(jumlah);
      if (isNaN(jumlah)) {
        console.log("❌ Jumlah harus angka");
        return nglSpamMenu();
      }
      if (jumlah > 500) {
        console.log("❌ Maksimal 500 spam per sekali kirim");
        return nglSpamMenu();
      }

      rl.question("Masukkan pesan: ", async (pesan) => {
        if (!pesan) return nglSpamMenu();

        console.log(`\n🚀 Memulai spam ke ${username}...`);
        console.log(`📝 Pesan: ${pesan}`);
        console.log(`🔢 Jumlah: ${jumlah}`);

        let sukses = 0, gagal = 0;
        for (let i = 1; i <= jumlah; i++) {
          try {
            const res = await axios.post(`https://ngl.link/api/submit`, {
              username: username,
              question: pesan,
              deviceId: Math.random().toString(36).substring(2, 18)
            }, {
              headers: {
                "Content-Type": "application/json"
              }
            });

            if (res.status === 200) {
              sukses++;
              console.log(`[${i}/${jumlah}] Berhasil mengirim ke ${username}`);
            } else {
              gagal++;
              console.log(`[${i}/${jumlah}] Gagal mengirim`);
            }
          } catch (e) {
            gagal++;
            console.log(`[${i}/${jumlah}] Error: ${e.message}`);
          }
          await new Promise(resolve => setTimeout(resolve, 500));
        }

        console.clear();
        console.log("\n✅ SPAM SELESAI");
        console.log("=".repeat(30));
        console.log(`👤 Username: ${username}`);
        console.log(`📝 Pesan: ${pesan}`);
        console.log(`✅ Sukses: ${sukses}`);
        console.log(`❌ Gagal: ${gagal}`);
        console.log("=".repeat(30));

        promptMenu();
      });
    });
  });
}

function otpSpamMenu() {
  console.clear();
  console.log("\n📱 OTP SPAM TOOL");
  rl.question("Masukkan nomor target (contoh: 8123456789): ", async (nomor) => {
    if (!nomor) return promptMenu();

    rl.question("Masukkan jumlah spam (max 10): ", async (jumlah) => {
      jumlah = parseInt(jumlah);
      if (isNaN(jumlah)) {
        console.log("❌ Jumlah harus angka");
        return otpSpamMenu();
      }
      if (jumlah > 10) {
        console.log("❌ Maksimal 10 spam per sekali kirim");
        return otpSpamMenu();
      }

      console.log(`\n🚀 Memulai OTP spam ke ${nomor}...`);
      console.log(`🔢 Jumlah: ${jumlah}`);

      let sukses = 0, gagal = 0;
      for (let i = 1; i <= jumlah; i++) {
        try {
          await sendOtpToNumber(nomor);
          sukses++;
          console.log(`[${i}/${jumlah}] Berhasil mengirim OTP`);
        } catch (e) {
          gagal++;
          console.log(`[${i}/${jumlah}] Gagal: ${e.message}`);
        }
        await new Promise(resolve => setTimeout(resolve, 1000));
      }

      console.clear();
      console.log("\n✅ OTP SPAM SELESAI");
      console.log("=".repeat(30));
      console.log(`📱 Nomor: ${nomor}`);
      console.log(`✅ Sukses: ${sukses}`);
      console.log(`❌ Gagal: ${gagal}`);
      console.log("=".repeat(30));

      promptMenu();
    });
  });
}

async function sendOtpToNumber(nomor) {
  const urls = [
    "https://api102.singa.id/new/login/sendWaOtp?versionName=2.4.8&versionCode=143&model=SM-G965N&systemVersion=9&platform=android&appsflyer_id=",
    "https://titipku.tech/v1/mobile/auth/otp?method=wa",
    "https://api-mobile.bisatopup.co.id/register/send-verification",
    "https://aci-user.bmsecure.id/v2/user/signin-otp/wa/send",
    "https://app.candireload.com/apps/v8/users/req_otp_register_wa",
    "https://sofia.bmsecure.id/central-api/sc-api/otp/generate",
    "https://keranjangbelanja.co.id/api/v1/user/otp",
    `https://irsx.mitradeltapulsa.com:8080/appirsx/appapi.dll/otpreg?phone=${nomor}`,
    "https://agenpayment-app.findig.id/api/v1/user/register",
    "https://agenpayment-app.findig.id/api/v1/user/login"
  ];

  const payloads = [
    JSON.stringify({ mobile_phone: nomor, type: "mobile", is_switchable: 1 }),
    JSON.stringify({ phone_number: "+62" + nomor, message_placeholder: "hehe" }),
    `phone_number=${nomor}`,
    JSON.stringify({ phone_user: nomor }),
    JSON.stringify({ uuid: "b787045b140c631f", phone: nomor }),
    JSON.stringify({ version_name: "6.2.1 (428)", phone: nomor }),
    `---dio-boundary-0879576676\r\ncontent-disposition: form-data; name="phone"\r\n\r\n${nomor}\r\n---dio-boundary-0879576676\r\ncontent-disposition: form-data; name="otp"\r\n\r\n118872\r\n---dio-boundary-0879576676--`,
    "",
    JSON.stringify({ name: "AAD", phone: nomor, pin: "1111", referral_code: "", password: "12345678" }),
    JSON.stringify({ phone: nomor, pin: "1111" })
  ];

  const headers = [
    { "Content-Type": "application/json; charset=utf-8" },
    { "Content-Type": "application/json; charset=UTF-8" },
    { "Content-Type": "application/x-www-form-urlencoded" },
    { "Content-Type": "application/json; charset=UTF-8" },
    { "Content-Type": "application/json" },
    { "Content-Type": "application/json" },
    { "content-type": "multipart/form-data; boundary=--dio-boundary-0879576676" },
    {},
    { "content-type": "application/json; charset=utf-8" },
    { "content-type": "application/json; charset=utf-8" }
  ];

  for (let i = 0; i < urls.length; i++) {
    let url = urls[i];
    let payload = payloads[i];
    let header = Object.assign({}, headers[i]);

    try {
      if (url.includes("aci-user.bmsecure.id")) {
        const jogjaTokenResp = await axios.post(
          "https://aci-user.bmsecure.id/oauth/token",
          "grant_type=client_credentials&uuid=00000000-0000-0000-0000-000000000000&id_user=0&id_kota=0&location=0.0%2C0.0&via=jogjakita_user&version_code=501&version_name=6.10.1",
          {
            headers: {
              "Authorization": "Basic OGVjMzRmMOGDctOTYyMC00YjQwLTg0YjQtYjM0ZTAwMDAwMDAw",
              "Content-Type": "application/x-www-form-urlencoded",
              "User -Agent": "okhttp/4.10.0"
            }
          }
        );
        const jogjaToken = jogjaTokenResp.data.access_token || "";
        header["Authorization"] = `Bearer ${jogjaToken}`;
      }

      if (url.includes("sofia.bmsecure.id")) {
        const speedcashTokenResp = await axios.post(
          "https://sofia.bmsecure.id/central-api/oauth/token",
          "grant_type=client_credentials",
          {
            headers: {
              "Authorization": "Basic NGFiY2M5ZmkzNWQxQTt3Y2Q0Y2Q0Y2Q0Y2Q0Y2Q0Y2Q0",
              "Content-Type": "application/x-www-form-urlencoded"
            }
          }
        );
        const speedcashToken = speedcashTokenResp.data.access_token || "";
        header["Authorization"] = `Bearer ${speedcashToken}`;
      }

      if (Object.keys(header).length > 0) {
        await axios.post(url, payload, { headers: header, timeout: 15000 });
      } else {
        await axios.post(url, payload, { timeout: 15000 });
      }

      console.log(`Mengirim OTP ke ${nomor} melalui ${url}...`);
    } catch (e) {
      console.log(`Error sending to ${url}: ${e.message}`);
    }
  }
}

// =================== Telegram Search ===================
async function getRealTelegramLink(joinUrl) {
  try {
    const { data } = await axios.get(joinUrl, {
      headers: {
        "User-Agent":
          "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36"
      },
      timeout: 15000
    });

    const $ = cheerio.load(data);
    const realLink = $('a[href^="tg://resolve"]').attr("href");

    if (realLink) {
      const username = realLink.split("tg://resolve?domain=")[1];
      return `https://t.me/${username}`;
    }
  } catch (e) {
    console.error(`💢 Gagal ambil link asli cuks: ${e.message}`);
  }
  return joinUrl; // fallback kalo gagal
}

async function searchTelegramChannels(query) {
  try {
    console.log(`Wett... lagi nyari channel "${query}" nih cuks...`);
    const url = `https://en.tgramsearch.com/search?query=${encodeURIComponent(query)}`;

    const { data } = await axios.get(url, {
      headers: {
        "User-Agent":
          "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36"
      },
      timeout: 20000
    });

    const $ = cheerio.load(data);
    const results = [];

    for (const el of $(".tg-channel-wrapper").toArray()) {
      const name = $(el).find(".tg-channel-link a").text().trim();
      let link = $(el).find(".tg-channel-link a").attr("href");
      const image = $(el).find(".tg-channel-img img").attr("src");
      const members = $(el).find(".tg-user-count").text().trim();
      const description = $(el).find(".tg-channel-description").text().trim();
      const category = $(el).find(".tg-channel-categories a").text().trim();

      // Biar link join jadi t.me
      if (link?.startsWith("/join/")) {
        link = await getRealTelegramLink(`https://en.tgramsearch.com${link}`);
      } else if (link?.startsWith("tg://resolve?domain=") || link?.includes("tg://")) {
        const username = link.split("tg://resolve?domain=")[1] || link.replace(/tg:\/\/?/, "");
        link = `https://t.me/${username}`;
      } else if (link && link.startsWith("/")) {
        link = `https://en.tgramsearch.com${link}`;
      }

      results.push({ name, link, image, members, description, category });
    }

    if (results.length === 0) {
      console.log("Waduh cuks, nggak nemu channelnya 😭");
    }

    return results;
  } catch (err) {
    console.error(`💢 Error scraping nye cuks: ${err.message}`);
    return [];
  }
}

// Telegram CLI menu
function telegramMenu() {
  console.clear();
  console.log("\n🔍 TELEGRAM CHANNEL SEARCH");
  rl.question("Masukkan kata kunci (contoh: musik, game): ", async (q) => {
    if (!q) return promptMenu();
    try {
      const res = await searchTelegramChannels(q);
      if (!res || res.length === 0) {
        console.log("❌ Tidak ada hasil.");
        return promptMenu();
      }
      console.log("\n🎯 Hasil Pencarian:");
      console.log("=".repeat(60));
      res.forEach((item, i) => {
        console.log(`${i + 1}. ${item.name || "(No name)"}`);
        console.log(`   👥 Members   : ${item.members || "-"}`);
        console.log(`   🏷️ Category : ${item.category || "-"}`);
        console.log(`   📝 Deskripsi : ${item.description || "-"}`);
        console.log(`   🔗 Link      : ${item.link || "-"}`);
        console.log("-".repeat(40));
      });
      // Tawarkan simpan hasil
      rl.question("\n🗃️ Simpan hasil ke file? (y/n): ", async (ans) => {
        if (ans.toLowerCase() === 'y') {
          const filename = `telegram_search_${sanitizeFilename(q)}.json`;
          await saveResultToFile(res, filename);
        }
        promptMenu();
      });
    } catch (e) {
      console.error("❌ Error:", e.message);
      promptMenu();
    }
  });
}

// =================== TempMail ===================
const tempmail = {
  api: {
    base: 'https://api.tempmail.lol',
    endpoints: {
      create: '/v2/inbox/create',
      inbox: token => `/v2/inbox?token=${token}`
    }
  },

  headers: {
    'user-agent': 'NB Android/1.0.0'
  },

  create: async (prefix = null) => {
    try {
      const payload = { domain: null, captcha: null };
      if (prefix) payload.prefix = prefix;

      const { data } = await axios.post(
        `${tempmail.api.base}${tempmail.api.endpoints.create}`,
        payload,
        { headers: tempmail.headers }
      );

      const createdAt = new Date();
      const expiresAt = new Date(createdAt.getTime() + (60 * 60 * 1000));

      return {
        success: true,
        code: 200,
        result: {
          address: data.address,
          token: data.token,
          expiresAt
        }
      };
    } catch (error) {
      return {
        success: false,
        code: error?.response?.status || 500,
        result: { error: error.message }
      };
    }
  },

  checkInbox: async (token, expiresAt) => {
    try {
      if (!token || token.trim() === '') {
        return {
          success: true,
          code: 200,
          result: {
            token: null,
            emails: [],
            expiresAt: expiresAt.toISOString()
          }
        };
      }

      let attempt = 0;
      let inboxnya;

      while (true) {
        if (new Date() > expiresAt) {
          return {
            success: false,
            code: 410,
            result: { error: 'Token buat email ini udah expired bree, kagak bisa diakses 😂', expiresAt }
          };
        }

        const { data } = await axios.get(
          `${tempmail.api.base}${tempmail.api.endpoints.inbox(token)}`,
          { headers: tempmail.headers }
        );

        if (data.expired) {
          return {
            success: false,
            code: 410,
            result: { error: 'Emailnya udah expired bree.. bikin tempmail yang baru aja gih... ' }
          };
        }

        if (data.emails?.length > 0) {
          inboxnya = data;
          break;
        }

        attempt++;
        await new Promise(resolve => setTimeout(resolve, 3000));
      }

      const emails = inboxnya.emails.map(e => ({
        id: e.id || e._id,
        from: e.from,
        to: e.to,
        subject: e.subject,
        body: e.body,
        createdAt: e.createdAt,
        attachments: e.attachments || []
      }));

      return {
        success: true,
        code: 200,
        result: {
          token,
          expired: inboxnya.expired,
          expiresAt: expiresAt.toISOString(),
          emails
        }
      };
    } catch (error) {
      return {
        success: false,
        code: error?.response?.status || 500,
        result: { error: error.message }
      };
    }
  }
};

// TempMail CLI
function tempmailMenu() {
  console.clear();
  console.log("\n📧 TEMPMAIL (Email Sementara)");
  console.log("1. ✨ Buat email sementara");
  console.log("2. 📥 Cek inbox (pakai token)");
  console.log("3. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      rl.question("\nMasukkan prefix (opsional, tekan enter kosong): ", async (pref) => {
        try {
          console.log("🔄 Membuat tempmail...");
          const res = await tempmail.create(pref ? pref.trim() : null);
          if (!res.success) {
            console.log("❌ Gagal:", res.result.error || "unknown");
            return tempmailMenu();
          }
          console.log("\n✅ Berhasil dibuat:");
          console.log(`📧 Alamat : ${res.result.address}`);
          console.log(`🔑 Token  : ${res.result.token}`);
          console.log(`⏳ Expires : ${res.result.expiresAt}`);
          // simpan ke file
          const filename = `tempmail_${sanitizeFilename(res.result.address)}.json`;
          await saveResultToFile(res.result, filename);
        } catch (e) {
          console.log("❌ Error:", e.message);
        }
        tempmailMenu();
      });
    } else if (choice === "2") {
      rl.question("\nMasukkan token: ", async (token) => {
        rl.question("Masukkan expiresAt (ISO string dari create) : ", async (exp) => {
          try {
            const expiresAt = new Date(exp);
            if (isNaN(expiresAt.getTime())) {
              console.log("❌ Format expiresAt salah. Pastikan ISO string.");
              return tempmailMenu();
            }
            console.log("🔄 Mengecek inbox...");
            const inbox = await tempmail.checkInbox(token.trim(), expiresAt);
            if (!inbox.success) {
              console.log("❌ Gagal:", inbox.result.error || "unknown");
            } else {
              console.log("\n📨 Inbox:");
              console.log(JSON.stringify(inbox.result.emails, null, 2));
              const filename = `tempmail_inbox_${sanitizeFilename(token)}_${Date.now()}.json`;
              await saveResultToFile(inbox.result, filename);
            }
          } catch (e) {
            console.log("❌ Error:", e.message);
          }
          tempmailMenu();
        });
      });
    } else if (choice === "3") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      tempmailMenu();
    }
  });
}

// =================== Luban (Verif SMS) ===================
const luban = {
  api: {
    base: 'https://lubansms.com',
    endpoints: {
      freeCountries: (lang = 'en') =>
        `/v2/api/freeCountries?language=${lang}`,
      freeNumbers: (countryName = 'russia') =>
        `/v2/api/freeNumbers?countries=${countryName}`,
      freeMessages: (countryName, number) =>
        `/v2/api/freeMessage?countries=${countryName}&number=${number}`
    }
  },

  headers: {
    'user-agent': 'NB Android/1.0.0',
    'accept-encoding': 'gzip',
    system: 'Android',
    time: `${Date.now()}`,
    type: '2'
  },

  request: async (countryName = '') => {
    if (typeof countryName !== 'string' || !countryName.trim()) {
      return {
        success: false,
        code: 400,
        result: { error: 'Negaranya mana bree? Aelah 🗿' }
      };
    }

    const url = `${luban.api.base}${luban.api.endpoints.freeNumbers(countryName)}`;

    try {
      const { data } = await axios.get(url, {
        headers: luban.headers,
        timeout: 15000
      });

      if (!data || data.code !== 0 || !Array.isArray(data.msg)) {
        return {
          success: false,
          code: 500,
          result: { error: `Data nokos ${countryName} kagak valid bree.. cari yang lain aja dah 😂` }
        };
      }

      const active = data.msg
        .filter(n => !n.is_archive)
        .map(n => ({
          full: n.full_number.toString(),
          short: n.number.toString(),
          code: n.code,
          country: n.country,
          age: n.data_humans
        }));

      return {
        success: true,
        code: 200,
        result: {
          total: active.length,
          numbers: active,
          created: new Date().toISOString()
        }
      };
    } catch (error) {
      return {
        success: false,
        code: error?.response?.status || 500,
        result: { error: 'Error bree 🤙🏻😏' }
      };
    }
  },

  checkMessages: async (countryName = '', number = '') => {
    if (typeof countryName !== 'string' || !countryName.trim() ||
        typeof number !== 'string' || !number.trim()) {
      return {
        success: false,
        code: 400,
        result: { error: 'Negara ama Nokosnya kagak boleh kosong.. input ulang.. ' }
      };
    }

    number = number.replace(/\D/g, '');

    const url = `${luban.api.base}${luban.api.endpoints.freeMessages(countryName, number)}`;

    try {
      const { data } = await axios.get(url, {
        headers: luban.headers,
        timeout: 15000
      });

      if (!data || typeof data !== 'object' || data.code !== 0 || !('msg' in data)) {
        return {
          success: false,
          code: 500,
          result: { error: 'Data pesannya kagak valid bree, coba lagi nanti aja yak :v' }
        };
      }

      const i = Array.isArray(data.msg) ? data.msg : [];

      const messages = i.map(m => ({
        id: m.id,
        from: m.in_number || m.innumber || '',
        to: m.my_number,
        text: m.text,
        code: m.code !== '-' ? m.code : null,
        received: m.created_at,
        age: m.data_humans
      }));

      return {
        success: true,
        code: 200,
        result: {
          total: messages.length,
          messages,
          created: new Date().toISOString()
        }
      };
    } catch (error) {
      return {
        success: false,
        code: error?.response?.status || 500,
        result: { error: error.message || 'Yaaakk... error bree 🤙🏻😏' }
      };
    }
  },

  generate: async (countryName = '') => {
    if (typeof countryName !== 'string' || !countryName.trim()) {
      return {
        success: false,
        code: 400,
        result: { error: 'Negaranya kagak boleh bree.. Input ulang kagak lu 🫵🏻' }
      };
    }

    try {
      const i = await axios.get(
        `${luban.api.base}${luban.api.endpoints.freeCountries()}`,
        {
          headers: luban.headers,
          timeout: 15000
        }
      );

      if (!i.data || i.data.code !== 0) {
        return {
          success: false,
          code: 500,
          result: { error: 'Daftar negaranya kagak bisa diambil bree, coba nanti lagi aja yak 🤙🏻😏' }
        };
      }

      const target = i.data.msg.find(
        c => c.name.toLowerCase() === countryName.toLowerCase()
      );

      if (!target) {
        return {
          success: false,
          code: 404,
          result: { error: `Negara ${countryName} mah kagak ada bree.. Cari yang lain aja dah.. ` }
        };
      }

      if (!target.online) {
        return {
          success: false,
          code: 403,
          result: { error: `Negara ${countryName} offline 🚫` }
        };
      }

      const res = await luban.request(countryName);
      if (!res.success) return res;

      const sorted = res.result.numbers.sort((a, b) => {
        const ageA = atom(a.age);
        const ageB = atom(b.age);
        return ageA - ageB;
      });

      return {
        success: true,
        code: 200,
        result: {
          total: sorted.length,
          numbers: sorted.map(n => ({
            ...n,
            countryName: target.locale
          })),
          created: new Date().toISOString()
        }
      };
    } catch (error) {
      return {
        success: false,
        code: error?.response?.status || 500,
        result: { error: 'Beuhh... error bree 🤙🏻😏' }
      };
    }
  }
};

function atom(text) {
  const map = {
    minute: 1,
    minutes: 1,
    hour: 60,
    hours: 60,
    day: 1440,
    days: 1440,
    week: 10080,
    weeks: 10080
  };

  const [value, unit] = (text || "").split(' ');
  return parseInt(value) * (map[unit] || 999999);
}

// Luban CLI menu
function lubanMenu() {
  console.clear();
  console.log("\n📱 VERIF SMS (Luban)");
  console.log("1. 🌐 Daftar negara (dapat dipakai)");
  console.log("2. 📥 Ambil nomor gratis dari negara (Data nomor berada di /sdcard/hasil-download bagian code)");
  console.log("3. 📨 Cek pesan untuk nomor");
  console.log("4. 🔙 Kembali ke Menu Utama");

  rl.question("Pilih opsi: ", (choice) => {
    if (choice === "1") {
      // list countries
      (async () => {
        try {
          const res = await axios.get(`${luban.api.base}${luban.api.endpoints.freeCountries()}`, { headers: luban.headers, timeout: 15000 });
          if (!res.data || !Array.isArray(res.data.msg)) {
            console.log("❌ Gagal ambil daftar negara.");
            return lubanMenu();
          }
          console.log("\n🌍 Daftar negara (sample):");
          res.data.msg.forEach(c => {
            console.log(`- ${c.name} (locale: ${c.locale}) ${c.online ? "🟢 online" : "🔴 offline"}`);
          });
        } catch (e) {
          console.log("❌ Error:", e.message);
        }
        lubanMenu();
      })();
    } else if (choice === "2") {
      rl.question("\nMasukkan nama negara (contoh: russia): ", async (country) => {
        if (!country) return lubanMenu();
        try {
          console.log("🔍 Mengambil nomor...");
          const res = await luban.generate(country.trim());
          if (!res.success) {
            console.log("❌ Gagal:", res.result.error || "unknown");
            return lubanMenu();
          }
          console.log("\n📞 Nomor tersedia:");
          console.log(JSON.stringify(res.result.numbers, null, 2));
          const filename = `Numbers_${sanitizeFilename(country)}.json`;
          await saveResultToFile(res.result.numbers, filename);
        } catch (e) {
          console.log("❌ Error:", e.message);
        }
        lubanMenu();
      });
    } else if (choice === "3") {
      rl.question("\nMasukkan nama negara (contoh: russia): ", (country) => {
        rl.question("Masukkan nomor (full atau short): ", async (number) => {
          if (!country || !number) return lubanMenu();
          try {
            console.log("🔎 Mengecek pesan...");
            const res = await luban.checkMessages(country.trim(), number.trim());
            if (!res.success) {
              console.log("❌ Gagal:", res.result.error || "unknown");
            } else {
              console.log("\n📨 Pesan diterima:");
              console.log(JSON.stringify(res.result.messages, null, 2));
              const filename = `Messages_${sanitizeFilename(country)}_${sanitizeFilename(number)}.json`;
              await saveResultToFile(res.result.messages, filename);
            }
          } catch (e) {
            console.log("❌ Error:", e.message);
          }
          lubanMenu();
        });
      });
    } else if (choice === "4") {
      promptMenu();
    } else {
      console.log("❌ Pilihan tidak valid");
      lubanMenu();
    }
  });
}

process.on('SIGINT', () => {
  console.log("\n\n👋 Program dihentikan");
  process.exit(0);
});
