# react-native-image-fillter


<a href="https://imgflip.com/gif/3pbthc"><img src="https://i.imgflip.com/3pbthc.gif" title="made at imgflip.com"/></a>





How to use



import React, {Component} from 'react';
import {Button, FlatList, Image, NativeModules, TouchableOpacity, View,Text} from 'react-native';
import ImagePicker from 'react-native-image-crop-picker';
import imageFillter from 'react-native-imagefillter'
const id =imageFillter
export default class App extends Component {
  constructor() {
    super()
    this.state = {
      imageSoure: "",
      imageArraySoure: []

    }
  }

  imageFillter = async (i) => {
    alert(JSON.stringify(id))
    let result = await new Promise((resolve, reject) => {
      id.getSourceImage({
        imageSource: this.state.imageSoure,
        dataType: "Path",// optional for  ios
        filterType: i
      }, (source) => {
        resolve(source.base64)
      });
    })
    return (result)
  }

  onPress = () => {
    ImagePicker.openPicker({
      width: 300,
      height: 400,
      cropping: true
    }).then(image => {
      this.setState({
        imageSoure: Platform.OS === 'ios' ? "file:///" + image.path : image.path
      }, () => {

      })
    })
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

  onImagePress = (FillterImage) => {
    this.setState({
      imageSoure: FillterImage
    })
  }

  render() {
    return (
        <View style={{flex: 1, alignItems: "center"}}>
          <Image source={{uri: this.state.imageSoure}} style={{height: 300, width: 300}}/>
          <Button title={"pressMe"} onPress={(() => this.onPress())}/>

          <Button title={"FilterImage"} onPress={(() => this.FilterImage())}/>

          <FlatList
              horizontal={true}
              data={this.state.imageArraySoure}
              renderItem={({item,index}) => (
                  <TouchableOpacity style={{alignItems:'center'}} onPress={() => this.onImagePress(item.FillterImage)
                  }>
                    <Image source={{uri: item.FillterImage}} style={{height: 100, width: 100}}
                           resizeMode={"contain"}/>

                    <Text style={{fontSize: 16, color:'red',}} numberOfLines={1}> {index}
                    </Text>
                  </TouchableOpacity>
              )}
          />
        </View>
    );
  }
}
