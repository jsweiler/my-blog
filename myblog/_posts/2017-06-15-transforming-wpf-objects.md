---
layout: post
title:  "Transforming WPF objects"
date:   2017-06-15 06:04:29 -0400
categories: wpf
---
#### Wpf Transformations
What is a transformation? In WPF transformations can be used to rotate, scale, skew or even move objects. For a nice overview of the various transformations [see here](https://docs.microsoft.com/en-us/dotnet/framework/wpf/graphics-multimedia/transforms-overview). 

I will be creating a little WPF app that shows how some of the transformations work. https://github.com/jsweiler/WpfTransforms

#### Getting started
I want to have a rectangle that you can rotate and scale using sliders. So with the rotate slider added it looks like this. 
![](https://jweiler.ghost.io/content/images/2017/06/rotate.PNG)
So far this is pretty simple. I handle the ValueChanged Event on the slider like this.

```csharp
private void Slider_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    rotateMatrix = new Matrix();
    rotateMatrix.Rotate(e.NewValue);

    matrixTransform.Matrix = rotateMatrix * scaleMatrix;
}

```
So there I am setting the rotateMatrix to a new `Matrix` and calling the `Rotate` method with the new value from the slider. The matrixTransform is the name of my transformation on the `Rectangle`.

```xaml
<Rectangle Grid.Column="1" Fill="Green" Margin="5">
    <Rectangle.RenderTransform>
        <MatrixTransform x:Name="matrixTransform"/>
    </Rectangle.RenderTransform>
</Rectangle>
```

So that rotates when you adjust the slider. But as you can see below it probably rotates differently that you would expect.
![](https://jweiler.ghost.io/content/images/2017/06/rotategif.gif)
That is because the center of rotation is in the top left corner. We can change that by setting the `RenderTransformOrigin` property on the Rectangle.

Now the rectangle looks like this in xaml.
```xaml
<Rectangle Grid.Column="1" Fill="Green" Margin="5"
           RenderTransformOrigin=".5 .5">
    <Rectangle.RenderTransform>
        <MatrixTransform x:Name="matrixTransform"/>
    </Rectangle.RenderTransform>
</Rectangle>
```
Now it rotates on the center.
![](https://jweiler.ghost.io/content/images/2017/06/correctRotate.gif)

#### Scaling
We now want to add a slider that scales the object. I make a new slider and handle the ValueChanged event like this.
```csharp
private void Scale_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
{
    scaleMatrix = new Matrix();
    scaleMatrix.Scale(e.NewValue, e.NewValue);

    matrixTransform.Matrix = rotateMatrix * scaleMatrix;
}
```
I use the same value for each parameter in the `Scale` method. That means the X and Y will scale the same, like this.
![](https://jweiler.ghost.io/content/images/2017/06/scale.gif)

#### Translate Transform
This transformation will allow you to adjust a slider for the X and Y positions of a Rectangle. For this one I don't handle the ValueChanged event on the slider, instead I just bind the X and Y to the value of the Slider in the `TranslateTransform`.
```xaml
<Rectangle Grid.Column="3" Fill="LightBlue" Margin="5"
           RenderTransformOrigin=".5 .5">
    <Rectangle.RenderTransform>
        <TranslateTransform Y="{Binding ElementName=ySlider, Path=Value}"
                            X="{Binding ElementName=xSlider, Path=Value}"/>
    </Rectangle.RenderTransform>
</Rectangle>
```
This is what it looks like.
![](https://jweiler.ghost.io/content/images/2017/06/translate.gif)

#### Summary
This was a brief overview of some of the different transformations you can do with WPF. Transformations are a very powerful tool and enable you to do many things to enhance and improve your application. For the final application see here https://github.com/jsweiler/WpfTransforms 

