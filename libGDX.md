## libGDX

### Downloads

## Guide

<table border=1>
 <tr>
 
 <td>libGDX Overview (1:46:20)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=ZrvYHt3efSQ" target="_blank">
<img src="http://img.youtube.com/vi/ZrvYHt3efSQ/0.jpg" alt="libGDX Overview" width="240" height="180" border="10" /></a>
 </td>
 
 <td></td>
</tr></table>

### Examples

Mario Zechner created a detailed post about running libGDX on OUYA - http://www.badlogicgames.com/wordpress/?p=2733

### Resources

libGDX - http://libgdx.badlogicgames.com/

# Intro #

`libGDX` uses `Java` and shares most of the [java documentation](java.md) for accessing the `OUYA SDK`.

# Button Data #

The Button controller text and images vary on each console. The `ButtonData` object gives access to the `String` and `Drawable` controller data.

## bitmap ##

The `ButtonData` image can also be converted to a `Texture` which can be used by `libGDX`. 

```
    public static Texture getButtonDataTexture(int button) {
        OuyaController.ButtonData buttonData = OuyaController.getButtonData(button);
        if (null == buttonData)
        {
            return null;
        }
        BitmapDrawable drawable = (BitmapDrawable)buttonData.buttonDrawable;
        if (null == drawable)
        {
            return null;
        }
        Bitmap bitmap = drawable.getBitmap();
        
        Texture tex = new Texture(bitmap.getWidth(), bitmap.getHeight(), Format.RGBA8888);
        GLES20.glBindTexture(GLES20.GL_TEXTURE_2D, tex.getTextureObjectHandle());
        GLUtils.texImage2D(GLES20.GL_TEXTURE_2D, 0, bitmap, 0);
        GLES20.glBindTexture(GLES20.GL_TEXTURE_2D, 0);
        bitmap.recycle();
        
        return tex;
    }
```
