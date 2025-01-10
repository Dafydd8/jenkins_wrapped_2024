<script>
  import * as d3 from "d3"
  import data1 from './data/Streaming_History_Audio_2023-2024_2.json'
  import data2 from './data/Streaming_History_Audio_2024-2025_3.json'
  import imagenes_artistas from './data/data_aux.csv'
  import imagenes_tracks from './data/data_aux2.csv'

  const nro_mes = {
    '01': 'Enero',
    '02': 'Febrero',
    '03': 'Marzo',
    '04': 'Abril',
    '05': 'Mayo',
    '06': 'Junio',
    '07': 'Julio',
    '08': 'Agosto',
    '09': 'Septiembre',
    '10': 'Octubre',
    '11': 'Noviembre',
    '12': 'Diciembre'
  }

  let total_mins = 0;
  let artistas = {};
  let tracks = {};
  let horas = {};
  let meses = {};

  for (let i = 0; i < data1.length; i++) {
    const reproduccion = data1[i];
    if (reproduccion.ts.substring(0,4) == '2024' && reproduccion['ms_played']/1000 >= 30){
      const mins = reproduccion['ms_played']/1000/60;
      const track = reproduccion['master_metadata_track_name'];
      const artista = reproduccion['master_metadata_album_artist_name'];
      const hora = reproduccion['ts'].substring(11, 13);
      const mes = reproduccion['ts'].substring(5, 7);
      total_mins += mins;
      if (!artistas[artista]){
        artistas[artista] = 0;
      } if (!tracks[track]){
        tracks[track] = 0;
      }
      if (!horas[hora]){
        horas[hora] = 0;
      } if (!meses[mes]){
        meses[mes] = 0;
      }
      artistas[reproduccion['master_metadata_album_artist_name']] += mins;
      tracks[reproduccion['master_metadata_track_name']] += mins; 
      horas[hora] += 1;
      meses[mes] += 1;
    }
  }
  for (let i = 0; i < data2.length; i++) {
    const reproduccion = data2[i];
    if (reproduccion['ts'].substring(0,4) == '2024' && reproduccion['ms_played']/1000 >= 30){
      const mins = reproduccion['ms_played']/1000/60;
      const track = reproduccion['master_metadata_track_name'];
      const artista = reproduccion['master_metadata_album_artist_name'];
      const hora = reproduccion['ts'].substring(11, 13);
      const mes = reproduccion['ts'].substring(5, 7);
      total_mins += mins;
      if (!artistas[artista]){
        artistas[artista] = 0;
      } if (!tracks[track]){
        tracks[track] = 0;
      }
      if (!horas[hora]){
        horas[hora] = 0;
      } if (!meses[mes]){
        meses[mes] = 0;
      }
      artistas[reproduccion['master_metadata_album_artist_name']] += mins;
      tracks[reproduccion['master_metadata_track_name']] += mins; 
      horas[hora] += 1;
      meses[mes] += 1;
    }
  }
  
  
  console.log('total mins', total_mins);
  let artistas_array = [];
  let tracks_array = [];

  let artistasList = Object.entries(artistas).sort((a, b) => b[1] - a[1]);
  console.log('Top 10 artistas: ', artistasList.slice(0, 10), '\n');

  let tracksList = Object.entries(tracks).sort((a, b) => b[1] - a[1]);
  console.log('Top 10 songs: ', tracksList.slice(0, 10), '\n');

  let horasList = Object.entries(horas).sort((a, b) => b[1] - a[1]);
  console.log('Horas: ', horasList, '\n');
  let mesesList = Object.entries(meses).sort((a, b) => b[1] - a[1]);
  console.log('Meses: ', mesesList, '\n');
</script>

<main>
  <div id="portada">
    <h1>Jenkins Spotify 2024</h1>
  </div>
  <div id="content">
    <div class="container">
      <h2>Resumen de reproducciones</h2>
      <p>Los Jenkins esucharon <span style="font-weight:bold; font-size:30px">{Math.floor(total_mins)}</span> minutos de música en el año</p>
      <p>El horario en que más escucharon fue a las {horasList[0][0]}hs, con {horasList[0][1]} reproducciones en ese horario en el año</p>
      <img src="./public/images/repr_dia.png" alt="graf1" class="graf"/>
      <p>El mes en que más escucharon fue {nro_mes[mesesList[0][0]]}, con {mesesList[0][1]} reproducciones en ese mes</p>
      <img src="./public/images/repr_mes.png" alt="graf2" class="graf"/>
    </div>
    <div class="container">
      <h2>Top 10 artistas</h2>
      <div id="item">
        <p style="font-size:40px;">#1</p>
        <img src={imagenes_artistas[0].profile_pic} alt="imagen" style="width:150px; border-radius:75px"/>
        <p style="font-size:40px">{artistasList[0][0]}</p>
      </div>
      <div class="listado_dos_columnas">
        <div class="columna">
          {#each artistasList.slice(1, 5) as par, i}
            <div id="item">
              <p>#{i+2}</p>
              <img src={imagenes_artistas[i+1].profile_pic} alt="imagen" style="width:50px; height:50px; object-fit:cover; border-radius:37.5px"/>
              <p>{par[0]}</p>
            </div>
          {/each}
        </div>
        <div class="columna">
          {#each artistasList.slice(5, 10) as par, i}
            <div id="item">
              <p>#{i+6}</p>
              <img src={imagenes_artistas[i+5].profile_pic} alt="imagen" style="width:50px; height:50px; object-fit:cover; border-radius:37.5px"/>
              <p>{par[0]}</p>
            </div>
          {/each}
        </div>
      </div>
    </div>  
      <div class="container">
      <h2>Top 10 canciones</h2>
      <div id="item">
        <p style="font-size:40px;">#1</p>
        <img src={imagenes_tracks[0].album_pic} alt="imagen" style="width:150px; border-radius:75px"/>
        <p style="font-size:40px">{tracksList[0][0]}</p>
      </div>
      <div class="listado_dos_columnas">
        <div class="columna">
          {#each tracksList.slice(1, 5) as par, i}
            <div id="item">
              <p>#{i+2}</p>
              <img src={imagenes_tracks[i+1].album_pic} alt="imagen" style="width:50px; height:50px; object-fit:cover; border-radius:37.5px"/>
              <p>{par[0]}</p>
            </div>
          {/each}
        </div>
        <div class="columna">
          {#each tracksList.slice(5, 10) as par, i}
            <div id="item">
              <p>#{i+6}</p>
              <img src={imagenes_tracks[i+5].album_pic} alt="imagen" style="width:50px; height:50px; object-fit:cover; border-radius:37.5px"/>
              <p>{par[0]}</p>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </div>

</main>

<style>
  #portada {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  #content {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 15px;
  }

  #item {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    gap: 15px;
  }

  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 15px;
    background-color: rgba(0, 0, 0, 0.5);
    width: 75vw;
    padding: 20px;
    margin: 20px;
    border-radius: 15px;
  }

  .grafs {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-evenly;
    gap: 15px;
  }
  .graf {
    width: 60%;
    height: auto;
  }

  .listado_dos_columnas {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content:space-evenly;
    width: 100%;
  }
  
  .columna {
    display: flex;
    flex-direction: column;
    align-items: top;
    justify-content: left;
    gap: 15px;
  }


  h1, h2 {
    font-family: 'Gotham', sans-serif;
    color: white;
    text-align: center;
  }

  p {
    font-family: 'Arial', sans-serif;
    color: white;
    font-weight: light;
    text-align: center;
    font-size: 20px;
  }

</style>