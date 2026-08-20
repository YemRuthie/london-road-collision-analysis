# DAX Measures

This file documents the principal DAX measures used in the **London Road Collision Analysis** Power BI dashboard.

## Collision Count

```DAX
Collision Count =
DISTINCTCOUNT('Data'[collision_index])
```

## Total Casualties

```DAX
Total Casualties =
SUM('Data'[number_of_casualties])
```

## Fatal Collisions

```DAX
Fatal Collisions =
CALCULATE(
    [Collision Count],
    'Data'[Severity] = "Fatal"
)
```

## Serious Collisions

```DAX
Serious Collisions =
CALCULATE(
    [Collision Count],
    'Data'[Severity] = "Serious"
)
```

## Slight Collisions

```DAX
Slight Collisions =
CALCULATE(
    [Collision Count],
    'Data'[Severity] = "Slight"
)
```

## KSI Collisions

KSI represents collisions classified as **Fatal or Serious**.

```DAX
KSI Collisions =
[Fatal Collisions] + [Serious Collisions]
```

## KSI Rate

```DAX
KSI Rate =
DIVIDE(
    [KSI Collisions],
    [Collision Count],
    0
)
```

## Average Casualties per Collision

```DAX
Average Casualties per Collision =
DIVIDE(
    [Total Casualties],
    [Collision Count],
    0
)
```

## Average Vehicles per Collision

```DAX
Average Vehicles per Collision =
DIVIDE(
    SUM('Data'[number_of_vehicles]),
    [Collision Count],
    0
)
```

## Previous Year Collisions

```DAX
Previous Year Collisions =
VAR CurrentYear = MAX('Data'[collision_year])
RETURN
    CALCULATE(
        [Collision Count],
        FILTER(
            ALL('Data'[collision_year]),
            'Data'[collision_year] = CurrentYear - 1
        )
    )
```

## Year-on-Year Collision Change

```DAX
YoY Collision Change % =
DIVIDE(
    [Collision Count] - [Previous Year Collisions],
    [Previous Year Collisions],
    0
)
```