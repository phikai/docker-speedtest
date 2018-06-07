# Docker Speedtest

This is a port of the Speedtest Docker Image from [Pedro César](https://github.com/pedrocesar-ti/internet-speedtest-docker/tree/docker_speedtest). A huge thanks to his legwork on the script and ideas.

## Purpose
This is a special purpose container designed to execute [Speedtest-CLI](https://github.com/sivel/speedtest-cli) and post the results to a companion InfluxDB Instance.

Image is based on Alpine Linux and available via [Docker Hub](https://hub.docker.com/r/phikai/speedtest/) as `phikai/speedtest`. This image is automatically rebuilt to a new `latest` when a new release to [Speedtest-CLI](https://github.com/sivel/speedtest-cli) is tagged. (based on Nico Maas' [idea](https://www.nico-maas.de/?p=1443)) 

## Usage
Container is designed to be used as a companion to a working InfluxDB Container. An example can be found [here](https://github.com/phikai/docker-internet-speedtest-dashboard).

```---
version: '2'
services:
  influxdb:
    image: influxdb 
    container_name: influxdb
    restart: unless-stopped
    network_mode: 'bridge'
    ports:
      - '8086:8086'
    environment:
      - INFLUXDB_DB=speedtest
    volumes:
      - './influxdb:/var/lib/influxdb'
  
  [...]
  
  speedtest:
    image: phikai/speedtest
    container_name: speedtest
    restart: unless-stopped
    network_mode: 'bridge'
    environment:
      - TEST_INTERVAL=5
    links:
      - influxdb:db
    depends_on:
      - influxdb
```

---

If this project has helped you in anyway, and you'd like to say thanks...

[![Donate](https://img.shields.io/badge/Donate-SquareCash-brightgreen.svg)](https://cash.me/$phikai)
[![Donate with Bitcoin](https://en.cryptobadges.io/badge/micro/15JCkpHhjjVmWYaTBc2fJn4tcKHEd194gY)](https://en.cryptobadges.io/donate/15JCkpHhjjVmWYaTBc2fJn4tcKHEd194gY)

---

# Disclaimer

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.