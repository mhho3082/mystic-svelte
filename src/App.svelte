<script lang="ts">
  let input = "";

  $: output = input.replace(/\D/g, "");

  $: links = [
    {
      href: `https://www.nhentai.net/g/${output}`,
      logo_url: "https://www.nhentai.net/favicon.ico",
      name: "nhentai",
    },
    {
      href: `https://www.wnacg.com/photos-index-aid-${output}.html`,
      logo_url:
        "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIIAAAAoCAYAAAAyhCJ1AAAACXBIWXMAAAsSAAALEgHS3X78AAAEv0lEQVR42u1by3GjQBBtb+3dxV50xyHgEHAIKAQUAoQgQoAQRAgiBBGCue9F1Eagvbyubc3CMAOMjFzzqijb/Lv7Tc/rHvxyu93Iw+OHd4GHJ4KHJ4KHJ4KHJ4KHJ4KHFj9NT/z95/fQ7hMRVUTUGNwiJaKaiHoHdkRE1C28d4Ct21iMrG3bve4emhFSIkrgPBOUuMYFSmxLcCSiT/xUCXIhoisR3Qy364q2nh36zT4jjDiuxSj/DjgQUUhEGYJ/wP4Mo7KyGJUJiFkZnJsZDKZYc05n+BwnRDgpzorB2v2Tk+EDtqWwbw9ytMJWEzQYybHBtJlOECEAESPN8ZCI8keLxQRbBQdxdpAvehlIl3yeuj/dGBn2sC0R7xZZTAs3kMAUb0T0S7P1RFRojrcgwiK8mK41QCyGCHJHRO/CSRcwssAoOOP3WiFHpaSxk3DGEvA7rZmRWKSVsPtgeW2JDNOMTAeZhYjtJ46TOKfbve7eXU8NZ/Hgq3iRDoGXyEfms1b83YI4W0SrBHYqIOqcrt5DorYQ2Snu02oydC8I1z5CI/R4UI+RHcHonJ4bIWwaCnQuspwNCg1xOuGzcIIUKc6vJ4iyKAa2RHgfKKsaR5VDZjFqWDAdDc+vlH7BSaT/eiBovxyS8NMwK+i01GL/LykfS6Gsz0J1rzVCM4vzA3GdaWYrlGrhDELkyrHUkpRqgKa0xcvE8Svep3CZEm3FonROCRLUcNQRoi+EU1uDDhkHbumIW0ssnsVUVwgh3CqiL4PdnbAjUYIV4/qDps5PDbKYqTbJ+TlzOotzMkKEl69FSiqwL3vyBhP3EbhZFg1MiUwE2VqPQYR8YDQHE/2GqWbQlFiU93rY1BDDUT0CfhSKmo/XI867wehCmZu3VjXsEWgmQu/oOeka9f+AqKxcEyEUWiAQhJBloa4D9kwohJYIMA12yuhOBYlD0TDj68KJtG6jO2z86pwIsok0thp2XZnlX4USQX6j+w6jWr8PBdc09b9tyeA5GuEEIzJRcp2F6Itpe21jW8RCCH44fhaX4cHM66s1+jhzNEIoDEhAhG5khDwjIrpfxOG2+pzpxaTkO+IZY6ubOrEYCYHePpIIkaILuJFTK3Mmp8lmQGdEI/fbCo5038nj9jmLu6ngcimZGBIhIP3qZgo/FiPxWJJNZvcRLqIiCKAJuE6+ijR1sRA3+QrNkrX6CIFo4OQjKZzt70cCw9PkwbCUPq2QTe8Wtx7RR4iE+OmFOuaRM1Z3c/m4RtBdIlOqBome/nUgP0G6RpCkREA7nGfzyZsuI5xBqGokHuUahtt8j8Bpv1acEwpjnrl0DJCGdV8iceXUIUBMiits30NI2373GC5454dXDSndL3eyYyQRnlkwHsVUpXN8IogSK8p9jt7hj2AuE75PNdmkeSQRmgFlKtfVK1rh27kBB4SGI8Nk9bEfGfEhniWXjhNRQYRKg4iXkXmdhQPVwU8N/f/thc6vL189CuYuOs3BHI1wcdCg+hgIkGwgMT7p30c3aoBppMGUCtKoA2avPM9V9vzYve7aLROBl3i39n8DLjWHFNK9ordcrbEUu9dd74wIHt8b/l/ePDwRPDwRPDwRPDwRPDwRPDwRPAzwF1qhg4ubfa+RAAAAAElFTkSuQmCC",
      name: "紳士漫畫",
    },
    {
      href: `https://www.pixiv.net/artworks/${output}`,
      logo_url: "https://s.pximg.net/www/images/accounts_index/logo.svg",
      name: "pixiv",
    },
    {
      href: `https://18comic.vip/album/${output}`,
      logo_url:
        "https://cdn-msp.jmcomic.me/media/logo/new_logo.png?v=2023110801",
      name: "禁漫天堂",
    },
  ];
</script>

<main>
  <div class="focus">
    <h1>Mystic Translator</h1>

    <input type="text" bind:value={input} />

    <div class="buttons">
      {#each links as link}
        <button
          class="button-link"
          on:click={() => {
            if (output.length) {
              window?.open(link.href, "_blank")?.focus();
            }
          }}
        >
          <img src={link.logo_url} alt={`Logo of ${link.name}`} />
          {link.name}
        </button>
      {/each}
    </div>
  </div>

  <footer>
    For personal use only.<br />
    All copyright and trademarks to their respective owners.<br />
    Inspired by
    <a href="https://d1074181045.github.io/mystic_transfer/" target="_blank">
      https://d1074181045.github.io/mystic_transfer/
    </a>
  </footer>
</main>

<style>
  main {
    height: 100%;
    max-height: 100%;
    margin: 0px auto;
    padding: 0px 5em;
    position: relative;

    display: flex;
    flex-direction: column;
    row-gap: 1em;
    justify-content: space-between;
    text-align: center;
  }

  .focus {
    height: 100%;
    max-height: 100%;
    width: 100%;
    margin: 0px auto;
    position: relative;

    display: flex;
    flex-direction: column;
    row-gap: 1em;
    align-items: center;
    justify-content: center;
    text-align: center;
  }

  h1 {
    margin: 0px auto;
  }

  input {
    width: clamp(25ch, 70%, 50ch);
    padding: 5px;
    font-size: 1.25em;
    text-align: center;

    background-color: #504945;
    border-color: #665c54;
    color: #fbf1c7;
  }

  .buttons {
    width: 100%;
    display: grid;
    column-gap: 10px;
    row-gap: 10px;
  }
  @media (min-width: 480px) {
    .buttons {
      grid-auto-flow: column;
      grid-template-columns: repeat(4, 1fr);
    }
  }
  @media (max-width: 480px) {
    .buttons {
      grid-auto-flow: row;
      grid-template-rows: repeat(4, 1fr);
    }
  }

  .button-link {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    background-color: #504945;
    border-color: #665c54;
    color: #fbf1c7;
    border-radius: 10px;

    padding: 5px;
    column-gap: 5px;
    row-gap: 5px;
  }

  .button-link img {
    max-width: 100px;
  }

  footer {
    justify-self: stretch;
    margin-bottom: 1em;
  }

  a {
    color: #83a598;
  }
</style>
