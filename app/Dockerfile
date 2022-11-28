# syntax=docker/dockerfile:1
# Chuẩn bị môi trường, Docker bắt đầu build từ "node:12-alpine" image
FROM node:12-alpine
RUN apk add --no-cache python2 g++ make
WORKDIR /app
COPY . .
# Tải các dependencies trong package.json
# Flag --production -> k tải các depen devDepen
RUN npm install --production
# Tạo instruct default khi tạo container từ image này
CMD ["node", "src/index.js"]
EXPOSE 3000
