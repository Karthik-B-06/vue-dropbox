<template>
  <div class="container">
    <div class="file-selector">
      <figure>
        <UploadIcon/>
      </figure>Select Files from Dropbox
      <p>
        <span>Authenticate with Dropbox</span>
      </p>
      <button type="button" @click="dropboxIconClicked();">Connect to Dropbox</button>
    </div>
    <AttachmentList :tempAttachments="getTempAttachments"/>
  </div>
</template>

<script>
import AttachmentList from "./AttachmentList";
import UploadIcon from "./UploadIcon";
export default {
  name: "DropboxSelector",
  components: {
    UploadIcon,
    AttachmentList
  },
  mounted() {
    let dropBox = document.createElement("script");
    dropBox.setAttribute(
      "src",
      "https://www.dropbox.com/static/api/2/dropins.js"
    );
    dropBox.setAttribute("id", "dropboxjs");
    dropBox.setAttribute("data-app-key", "0xnzzt59sd7mdib");
    document.head.appendChild(dropBox);
  },
  computed: {
    getTempAttachments() {
      return this.tempAttachments;
    }
  },
  data() {
    return {
      tempAttachments: []
    };
  },
  methods: {
    dropboxIconClicked() {
      let options = {
        // Required. Called when a user selects an item in the Chooser.
        success: async files => {
          let attachments = [];
          for (let i = 0; i < files.length; i++) {
            let attachment = {};
            attachment._id = files[i].id;
            attachment.title = files[i].name;
            attachment.size = files[i].bytes;
            attachment.iconURL = files[i].icon;
            attachment.link = files[i].link;
            attachment.extension = `. ${files[i].name.split(".")[1]}`;
            attachments.push(attachment);
          }
          this.tempAttachments = attachments;
          console.log(this.tempAttachments);
        },
        // Optional. Called when the user closes the dialog without selecting a file
        // and does not include any parameters.
        cancel: function() {},

        // Optional. "preview" (default) is a preview link to the document for sharing,
        // "direct" is an expiring link to download the contents of the file. For more
        // information about link types, see Link types below.
        linkType: "preview", // or "direct"

        // Optional. A value of false (default) limits selection to a single file, while
        // true enables multiple file selection.
        multiselect: true, // or true

        // Optional. This is a list of file extensions. If specified, the user will
        // only be able to select files with these extensions. You may also specify
        // file types, such as "video" or "images" in the list. For more information,
        // see File types below. By default, all extensions are allowed.
        extensions: [
          ".pdf",
          ".doc",
          ".docx",
          ".jpg",
          ".png",
          ".jpeg",
          ".svg",
          ".psd",
          ".sketch"
        ],

        // Optional. A value of false (default) limits selection to files,
        // while true allows the user to select both folders and files.
        // You cannot specify `linkType: "direct"` when using `folderselect: true`.
        folderselect: false, // or true

        // Optional. A limit on the size of each file that may be selected, in bytes.
        // If specified, the user will only be able to select files with size
        // less than or equal to this limit.
        // For the purposes of this option, folders have size zero.
        sizeLimit: 102400000 // or any positive number
      };
      Dropbox.choose(options);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.file-selector {
  padding: 55px;
  font-weight: 600;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 4px;
  color: #4e5b69;
  z-index: -9;
}
figure {
  margin: 0px;
}

.dropzone-container {
  display: flex;
  flex-direction: column;
  border: 1px dashed #ccc;
  border-radius: 15px;
}
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
button {
  background: #031629;
  box-shadow: 0 0 2px 0 rgba(3, 22, 41, 0.11),
    0 6px 16px 0 rgba(3, 22, 41, 0.08);
  font-family: SFProDisplay-Regular;
  font-size: 14px;
  color: #ffffff;
  letter-spacing: 0.4px;
  padding: 12px 30px;
  border-radius: 4px;
  outline: none;
  cursor: pointer;
  transition: all 0.25s;
}

button:hover {
  background-color: #1587ea;
  border-color: transparent;
}
.separator {
  position: relative;
}
.separator:after {
  position: absolute;
  content: "";
  height: 1px;
  width: 200px;
  background: #d8d8d8;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
span {
  position: relative;
  background: #f9f9f9;
  padding: 0 4px;
  z-index: 9;
  font-size: 12px;
  color: #4e5b69;
}
</style>
