# percentile-rank-repo
#Import Libraries

        import pandas as pd
        import numpy as np
        import math
        from scipy import stats
        import matplotlib.pyplot as plt
        from mplsoccer import PyPizza,FontManager

# Setting up DataFrame & Calculating Values and Parameters


        df = pd.read_csv('#filepath')
        df = df.loc[(df['Pos']=='MF') & (df['90s']>=5)] 
        df = df.drop('Pos',axis=1)
        params = list(df.columns)
        params = params[2:]
        player_name = '#Player Name' # Confirm Spellings from FBref
        player = df.loc[df['Player']==player_name].reset_index()
        player = list(player.loc[0])
        player = player[3:]
    
        values = []
        for x in range(len(params)):
            values.append(math.floor(stats.percentileofscore(df[params[x]],player[x])))
            
    
# Code for Chart - MPLsoccer
      font_normal = FontManager('https://raw.githubusercontent.com/google/fonts/main/apache/roboto/'
                              'Roboto%5Bwdth,wght%5D.ttf')
        font_italic = FontManager('https://raw.githubusercontent.com/google/fonts/main/apache/roboto/'
                              'Roboto-Italic%5Bwdth,wght%5D.ttf')
        font_bold = FontManager('https://raw.githubusercontent.com/google/fonts/main/apache/robotoslab/'
                            'RobotoSlab%5Bwght%5D.ttf')
    
        # color for the slices and text
        slice_colors = ["#1A78CF"] * 5 + ["#FF9300"] * 4 + ["#D70232"] * 4
        text_colors = ["#000000"] * 10 + ["#F2F2F2"] * 3
    
        baker = PyPizza(
            params=params,                  # list of parameters
            background_color="#222222",     # background color
            straight_line_color="#000000",  # color for straight lines
            straight_line_lw=1,             # linewidth for straight lines
            last_circle_color="#000000",    # color for last line
            last_circle_lw=3,               # linewidth of last circle
            other_circle_lw=0,              # linewidth for other circles
            inner_circle_size=10            # size of inner circle
        )
    
        # plot pizza
        fig, ax = baker.make_pizza(
            values,                          # list of values
            figsize=(8, 8.5),                # adjust the figsize according to your need
            color_blank_space="same",        # use the same color to fill blank space
            slice_colors=slice_colors,       # color for individual slices
            value_colors=text_colors,        # color for the value-text
            value_bck_colors=slice_colors,   # color for the blank spaces
            blank_alpha=0.4,                 # alpha for blank-space colors
            kwargs_slices=dict(
                edgecolor="#000000", zorder=2, linewidth=2
            ),                               # values to be used when plotting slices
            kwargs_params=dict(
                color="#F2F2F2", fontsize=11,
                fontproperties=font_normal.prop, va="center"
            ),                               # values to be used when adding parameter labels
            kwargs_values=dict(
                color="#F2F2F2", fontsize=11,
                fontproperties=font_normal.prop, zorder=3,
                bbox=dict(
                    edgecolor="#000000", facecolor="cornflowerblue",
                    boxstyle="round,pad=0.2", lw=1
                )
            )                                # values to be used when adding parameter-values labels
        )
    
        # add title
        fig.text(
            0.515, 0.975, f"{player_name} Percentile Ranking", size=16,
            ha="center", fontproperties=font_bold.prop, color="#F2F2F2"
        )
    
        # add subtitle
        fig.text(
            0.515, 0.95,
            "Percentile Rank vs Top-Five League Midfielders | Season 2023-24",
            size=13,
            ha="center", fontproperties=font_bold.prop, color="#F2F2F2"
        )
    
        # add credits
        CREDIT_1 = "data: opta via FBref"
        CREDIT_2 = "@athalakbar13"
    
        fig.text(
            0.99, 0.02, f"{CREDIT_1}\n{CREDIT_2}", size=9,
            fontproperties=font_italic.prop, color="#F2F2F2",
            ha="right"
        )
    
        # add text
        fig.text(
            0.34, 0.925, "Progression       Passing      Defending", size=14,
            fontproperties=font_bold.prop, color="#F2F2F2"
        )
    
        # add rectangles
        fig.patches.extend([
            plt.Rectangle(
                (0.31, 0.9225), 0.025, 0.021, fill=True, color="#1a78cf",
                transform=fig.transFigure, figure=fig
            ),
            plt.Rectangle(
                (0.49, 0.9225), 0.025, 0.021, fill=True, color="#ff9300",
                transform=fig.transFigure, figure=fig
            ),
            plt.Rectangle(
                (0.615, 0.9225), 0.025, 0.021, fill=True, color="#d70232",
                transform=fig.transFigure, figure=fig
            ),
        ])
    
    
        plt.show()
