# react-native-image-fillter


<a href="https://imgflip.com/gif/3pbthc"><img src="https://i.imgflip.com/3pbthc.gif" title="made at imgflip.com"/></a>




Follow instructions  from App.js




    imageFillter = async (i) => {
        let result = await new Promise((resolve, reject) => {
            imageFillter.getSourceImage({
                imageSource: this.state.imageSoure,
                dataType: "Path",// optional for  ios
                filterType: i
            }, (source) => {
                resolve(source.base64)
            });
        })
        return (result)
    }



    FilterImage = async () => {
        if (!this.state.imageSoure) {
            alert("Please choose image")
            return
        } else {
            let tempArray: Array<any> = [];
            for (let i = 0; i < 21; i++) {
                let data = await this.imageFillter(i);
                tempArray.push({"FillterImage": Platform.OS === 'ios' ? data : "file://" + data})
            }
            this.setState({
                imageArraySoure: tempArray
            }, () => {

            })
        }
    }

   
