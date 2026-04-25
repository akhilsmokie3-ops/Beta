# fluffy-robot
Character Model for the World Wide Web: String Matching and Searching
// quantum-notes-stream.cjs
const fs = require("node:fs");
const { Markitdown } = require("@microsoft/markitdown"); // API: Markitdown class with render()[cite:2]

async function renderFileStream(inputPath, outputPath) {
  const md = new Markitdown();

  const chunks = [];
  const readStream = fs.createReadStream(inputPath, { encoding: "utf8" });

  for await (const chunk of readStream) {
    chunks.push(chunk);
  }

  const markdown = chunks.join("");
  const result = await md.render(markdown); // Async render, similar to client.chat.completions.create()[cite:2]

  await fs.promises.writeFile(outputPath, result.html, "utf8");
  return { bytesIn: Buffer.byteLength(markdown), bytesOut: Buffer.byteLength(result.html) };
}

async function main() {
  try {
    const res = await renderFileStream("quantum-notes.md", "quantum-notes.html");
    console.log("Done:", res);
  } catch (err) {
    console.error("Render error:", err);
  }
}

main();
