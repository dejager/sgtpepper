#include <metal_stdlib>
using namespace metal;

kernel void enjoyTheShow(texture2d<float, access::write> o[[texture(0)]],
                         constant float &time [[buffer(0)]],
                         constant float2 *touchEvent [[buffer(1)]],
                         constant int &numberOfTouches [[buffer(2)]],
                         ushort2 gid [[thread_position_in_grid]]) {

  int width = o.get_width();
  int height = o.get_height();
  float2 res = float2(width, height);
  float2 p = float2(gid.xy);

  float strength = 0.4;
  float t = time / 2.0;

  float3 col = float3(0);

  for(int i = -1; i <= 1; i++) {
    for(int j = -1; j <= 1; j++) {

      p += float2(i, j) / 3.0;
        float2 pos = p / res.xy;

        pos.y /= res.x / res.y;
        pos = 4.0 * (float2(0.5) - pos);

        for(float k = 1.0; k < 7.0; k += 1.0) {
          pos.x -= strength * sin(2.0 * t + k * 1.5 * pos.y) + t * 0.5;
          pos.y += strength * cos(2.0 * t + k * 1.5 * pos.x);
        }
      col += 0.5 + 0.5 * cos(time + pos.xyx + float3(0.0, 2.0, 4.0));
      }
  }

  col /= 9.0;

  //gamma
  col = pow(col, float3(0.4545));

  float4 color = float4(col, 1.0);

  o.write(color, gid);
}
