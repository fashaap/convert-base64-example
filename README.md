
# Cara convert image to base64 dan sebaliknya



```javascript

import { useState } from "react";
import axios from "axios"
function App() {
  const [postImage, setPostImage] = useState({ myFile: "" });

  const url = "https://localhost:3000/uploads"

  const createPost = async (newImage) => {
    try {
      await axios.post(url, newImage)
    } catch (error) {
      console.log(error);
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    createPost(postImage);
    
  };

  const handleFileUpload = async (e) => {
    const file = e.target.files[0];
    const base64 = await convertToBase64(file);
    setPostImage({ myFile: base64 });
    //setPostImage({ ...postImage, myFile: base64 });
    //console.log({myFile : base64});
  };

  function convertToBase64(file) {
    return new Promise((resolve, reject) => {
      const fileReader = new FileReader();
      fileReader.readAsDataURL(file);
      fileReader.onload = () => {
        resolve(fileReader.result);
      };
      fileReader.onerror = (error) => {
        reject(error);
      };
    });
  }

  return (
    <>
      <div>
        <form onSubmit={handleSubmit}>
          <input
            type="file"
            label="image"
            name="myFile"
            id="file-upload"
            accept=".jpg, .jpeg, .png"
            onChange={(e) => handleFileUpload(e)}
          />
        </form>
        <div>
          <img src={postImage.myFile} alt="" />
        </div>
      </div>
    </>
  );
}

export default App;

```
