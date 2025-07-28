FROM node:12 as scoreboard

RUN git clone https://github.com/HackerDom/ctf-scoreboard-client.git /repo
WORKDIR /repo/scoreboard

RUN npm install
RUN npm run build

FROM ghcr.io/hackerdom/checksystem@sha256:996e282708662ce198ab1d34554fc4646b13730cfe598db8afffc620a5e49b17

COPY --from=scoreboard /repo/scoreboard/build /scoreboard
ENV CS_STATIC=/scoreboard

# Install checker's dependencies for current CTF.
# For example, you can use Gornilo library for simple
# way to write checkers (https://github.com/HackerDom/Gornilo)

RUN apt-get update
RUN apt-get install -y --no-install-recommends python3 python3-pip
RUN pip3 install gornilo requests jsonschema --break-system-packages


# Copy checkers to /app/checkers catalog
# COPY checkers /app/checker
