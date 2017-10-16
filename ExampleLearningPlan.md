# Example Learning Plan

## Goal

I want to draw some UI text in a circle.

## Self assessment

I already know how to create UI text in a straight line. And I know how to see the mesh created. (By choosing wireframe mode in the scene view.) I also know a formula for turning a radius and an angle into X and Y coordinates and vice versa.

## Resources

I will search online for resources explaining how to modify the mesh created by the UI text objects.

## Reflection

After hunting around online I found a reference to this section of the scripting reference:
https://docs.unity3d.com/ScriptReference/UI.BaseMeshEffect.html

The documentation and forums are unclear over whether to use the Mesh or VertexHelper versions of this function. The documentation says use the Mesh, but the forums say to use VertexHelper. I suspect the documentation is out of date.

But either way, by deriving a class from the BaseMeshEffect and overriding the ModifyMesh function I can get access to the text mesh created by the UI Text object.

Here is the code to take the horizontal text and wrap it around a circle:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class CircleText : BaseMeshEffect
{
    public override void ModifyMesh(VertexHelper vertexHelper) 
    {
        int count = vertexHelper.currentVertCount;
        if (IsActive() && count > 0)
        {
            int pointsInGlyph = 4;
            for (int index = 0; index < count; index += pointsInGlyph)
            {
                Vector3 centre = Vector3.zero;

                UIVertex uiVertex = new UIVertex();
                for (int j = 0; j < pointsInGlyph; ++j)
                {
                    vertexHelper.PopulateUIVertex(ref uiVertex, index + j);
                    centre += uiVertex.position;
                }

                centre = centre / (float)pointsInGlyph;
                float distance = centre.x;
                float radius = centre.y;
                if (radius > 0.0f)
                {
                    float angle = distance / radius; // angle in radians

                    Matrix4x4 translate = Matrix4x4.Translate(Vector3.right * -centre.x);
                    Matrix4x4 rotate = Matrix4x4.Rotate(Quaternion.Euler(Vector3.forward * -angle * Mathf.Rad2Deg));
                    Matrix4x4 combined = rotate * translate;

                    for (int j = 0; j < pointsInGlyph; ++j)
                    {
                        vertexHelper.PopulateUIVertex(ref uiVertex, index + j);
                        uiVertex.position = combined.MultiplyPoint3x4(uiVertex.position);
                        vertexHelper.SetUIVertex(uiVertex, index + j);
                    }
                }
            }
        }
    }    
}
```

I've made a Unity project which tests this script:
https://github.com/LSBUGPG/CircleText
